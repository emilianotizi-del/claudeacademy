# A10 — Debugging: quando il sistema fa una cosa che nessuno capisce

**Corso 3 · Parte C · v1.0** — *modulo nuovo: è il punto in cui la committenza assistita si rompe davvero*

Prima o poi succede: il sistema fa una cosa inspiegabile, il codice non l'hai scritto tu, e il modello che l'ha scritto propone una correzione dopo l'altra senza risolvere. **È il momento in cui scopri se sei un architetto o un utente fortunato.**

Il debugging è l'unica attività in cui l'AI ti aiuta *meno* di quanto ti aspetteresti, per una ragione strutturale: **un bug è una discrepanza tra ciò che credi e ciò che è**, e il modello condivide le tue credenze — gliele hai date tu.

---

## 1. Il metodo scientifico, applicato

Il debugging non è "provare cose". È **restringere lo spazio delle ipotesi**, un esperimento alla volta.

### I cinque passi

**1 · Riproduci.** Un bug che non sai riprodurre non lo puoi risolvere: puoi solo sperare che sparisca. Prima di ogni altra cosa: *quali passi esatti, con quali dati, producono il comportamento?* Se è intermittente, è quasi sempre una di tre cose: **concorrenza** (due cose in parallelo), **stato residuo** (qualcosa è rimasto da prima), **dipendenza dal tempo** o dall'ordine.

**2 · Osserva, non indovinare.** La differenza tra un professionista e un dilettante è tutta qui. Il dilettante ipotizza e cambia codice. Il professionista **guarda**: log, valori reali, cosa arriva e cosa esce.

**3 · Formula ipotesi falsificabili.** Non "forse è il database". Ma: *"ipotizzo che la funzione X riceva un valore nullo quando l'utente non ha una quota. Se è vero, aggiungendo un log in ingresso vedrò `null`. Se vedo un numero, l'ipotesi è falsa."*

**4 · Bisezione.** È lo strumento più potente e il meno usato. Il bug è **da qualche parte** tra l'input corretto e l'output sbagliato: **taglia a metà**. Il dato è giusto a metà strada? Allora il problema è nella seconda metà. Ripeti. Cinque bisezioni riducono cento posti possibili a tre.
La stessa tecnica sulla **storia**: se funzionava la settimana scorsa e ora no, `git bisect` trova il commit colpevole in un tempo logaritmico. Su una storia di 200 commit: **otto tentativi**.

**5 · Correggi la causa, non il sintomo.** E poi — sempre — **scrivi il test che avrebbe catturato il bug**. Un bug corretto senza test è un bug che tornerà.

---

## 2. Il debugging con un agente: cosa funziona e cosa no

### Cosa funziona benissimo
- **Spiegare cosa fa un pezzo di codice** che non capisci.
- **Generare log strategici** ("aggiungi log a tutti i punti in cui questo valore viene modificato") — noioso per un umano, istantaneo per un agente.
- **Costruire un caso di riproduzione minimo** partendo da uno complicato.
- **Elencare ipotesi** che non ti erano venute in mente.
- **Bisezione automatica**: ha la pazienza che tu non hai.

### Cosa funziona male
- **Trovare la causa senza dati.** Se non gli dai osservazioni (log, valori reali, messaggi di errore completi), **inventa una causa plausibile e la corregge**. Il codice cambia, il bug resta, e ora hai due problemi.
- **Il loop di riparazione** (→ A4): correzione, non funziona, altra correzione, non funziona. Ogni iterazione aggiunge modifiche che nessuno ha chiesto, il codice degrada, e la causa originale è sempre lì sotto tre strati di pezze.

### La regola che li separa

> **Non chiedere mai "risolvi questo bug". Chiedi "aiutami a scoprire perché accade".**

La prima formulazione ottiene una correzione plausibile. La seconda ottiene una diagnosi. Solo la seconda produce conoscenza — e la conoscenza è ciò che ti impedisce di riavere lo stesso bug con un'altra faccia.

---

## 3. Il protocollo di diagnosi

Quando qualcosa non va, **prima di toccare codice**:

```
Ho un problema. NON scrivere codice, NON proporre correzioni.

FATTI OSSERVATI:
- Cosa dovrebbe accadere: [X]
- Cosa accade davvero: [Y]
- Passi per riprodurre: [1, 2, 3]
- Messaggio di errore completo: [incollato per intero, non riassunto]
- Cosa è cambiato di recente: [ultimo deploy / ultima modifica]
- È sempre riproducibile, o intermittente?

Il tuo compito:
1. Elenca le 5 ipotesi più plausibili sulla causa, ordinate per probabilità.
2. Per ciascuna: quale osservazione la CONFERMEREBBE e quale la ESCLUDEREBBE.
3. Dimmi qual è l'esperimento più economico che elimina più ipotesi in una
   volta sola.
4. Dimmi quali informazioni ti mancano per essere più preciso.

Non implementare nulla: scelgo io l'esperimento.
```

Il punto 3 è quello che accelera davvero: **l'esperimento buono non è quello che conferma l'ipotesi preferita, è quello che dimezza lo spazio delle possibilità** — indipendentemente dall'esito.

---

## 4. Le cinque famiglie di bug e come si riconoscono

| Sintomo | Famiglia probabile | Prima cosa da guardare |
|---|---|---|
| Funziona a volte sì a volte no, senza schema | **Concorrenza** o stato residuo | Cosa succede se due azioni avvengono insieme? C'è una cache? |
| Funziona in locale, non in produzione | **Configurazione** o dati | Variabili d'ambiente, versioni, e **i dati veri sono più sporchi dei tuoi** |
| Funzionava, ora no | **Regressione** | `git bisect`. Non pensare: bisezione. |
| Rallenta col tempo, poi si blocca | **Perdita di risorse** (memoria, connessioni) | Qualcosa viene aperto e mai chiuso |
| I numeri non tornano | **Doppia verità** o operazione non idempotente | Dove sono le due copie del dato? (→ A2, A7) |

**La riga più preziosa è la seconda.** "Funziona in locale" è quasi sempre il segnale che **i tuoi dati di prova sono troppo puliti**: mancano gli accenti, i campi nulli, i duplicati, i nomi da 200 caratteri, le email malformate. La realtà è sporca, e il codice generato dall'AI è ottimizzato per la realtà pulita del tuo esempio.

---

## 5. Osservabilità: rendere il futuro debuggabile

Il debugging in produzione è possibile **solo se hai preparato il terreno prima**. Tre strumenti, in ordine di importanza:

**1 · Log strutturati.** Non `console.log("qui")`. Ma eventi con un identificativo di richiesta, il momento, l'utente, l'azione, l'esito. **La regola d'oro:** ogni operazione che tocca dati deve poter essere ricostruita dai log — *chi ha fatto cosa, quando, con quale esito*. Attenzione: **i log non contengono mai dati personali non necessari né segreti** (→ A9); un log troppo generoso è una violazione in attesa.

**2 · Tracciamento degli errori.** Uno strumento che raccoglie automaticamente le eccezioni con il contesto. Costa poco, e la differenza è tra "un cliente ci ha detto che non funziona" e "sappiamo che è successo 14 volte, a queste ore, con questo input".

**3 · Allarmi sulle invarianti.** Non solo "il server è caduto", ma: *"esistono quote pagate senza socio corrispondente"*, *"il numero di iscritti è diminuito senza cancellazioni"*. **Le condizioni che non dovrebbero mai verificarsi vanno monitorate**, perché quando si verificano hai già un problema che sta silenziosamente corrompendo i dati.

> **La domanda che rende un sistema debuggabile:** *se questa cosa andasse storta alle 3 di notte, quali tracce lascerebbe?* Se la risposta è "nessuna", non hai un sistema: hai una scatola nera che per ora si comporta bene.

---

## 6. Il costo del debugging opaco

Un bug in codice **che comprendi** costa: capire → correggere → testare. Un bug in codice **che non comprendi** costa: capire il codice → capire il bug → correggere → **verificare di non aver rotto altro, perché non conosci le dipendenze**.

Questa differenza è il vero prezzo del codice generato che non hai mai letto. **Non lo paghi il giorno in cui lo generi. Lo paghi il giorno in cui si rompe** — e lo paghi con gli interessi, perché quel giorno hai fretta.

Da qui la disciplina, che è l'unica difesa vera: **il codice che tocca le zone critiche (soldi, permessi, dati) lo leggi quando viene scritto, non quando esplode.**

---

## Strumenti di questo modulo

1. **I cinque passi**: riproduci, osserva, ipotizza in modo falsificabile, biseca, correggi la causa.
2. **La bisezione**, sullo spazio e sulla storia (`git bisect`).
3. **Il protocollo di diagnosi** — "non scrivere codice, dammi cinque ipotesi e l'esperimento più economico".
4. **La tabella sintomo → famiglia di bug.**
5. **I tre strumenti di osservabilità**, incluso l'allarme sulle invarianti.
6. **La regola dell'oro:** ogni bug corretto diventa un test.

---

## Errori tipici

1. **Chiedere "risolvi" invece di "diagnostica".** Ottieni una pezza plausibile su una causa immaginaria.
2. **Insistere nel loop di riparazione.** Fermati alla seconda correzione fallita.
3. **Correggere senza riprodurre.** Non saprai mai se hai risolto o se il bug si è nascosto.
4. **Dati di prova troppo puliti.** La realtà ha accenti, nulli, duplicati e maiuscole a caso.
5. **Nessun log.** Il giorno dell'incidente è tardi per aggiungerli.
6. **Correggere il sintomo.** Il bug tornerà, con un'altra faccia, in un momento peggiore.

*v1.0 — modulo nuovo.*
