# A1 — Dirigere lo sviluppo (invece di scriverlo)

**Codice:** A1 · **Versione:** 0.1 · 13/07/2026 · **Corso 3 · Parte A**
**Dipendenze:** F1–F6 · ARCH-02 v0.2 · **Progetto:** Giano
**Deliverable di questo modulo:** la **carta del progetto Giano** (una pagina)

---

## 1. Obiettivo

Al termine di questo modulo avrai: chiaro **quale ruolo occupi** nello sviluppo di Giano; le **tre domande** da farti prima di ogni progetto; il **confine** oltre il quale non si va da soli — scritto, non intuito. E avrai prodotto la carta del progetto: il documento che, nei mesi difficili, ti dirà se stai ancora facendo la cosa giusta.

---

## 2. Il ruolo che occupi davvero

In un progetto software classico ci sono tre figure:

| Figura | Cosa fa | Chi la occupa in Giano |
|---|---|---|
| **Committente** | Sa *perché* il sistema esiste, decide le priorità, dice quando è fatto | **Tu** |
| **Architetto** | Decide *come* è fatto: struttura, dati, scelte di fondo, confini | **Tu, con Claude** — è la novità |
| **Esecutore** | Scrive il codice, riga per riga | **Claude Code** — e, per le parti critiche, un professionista |

**[FATTO storico]** Fino a poco fa la casella "architetto" era inaccessibile a chi non programmava: le decisioni strutturali erano di fatto delegate all'esecutore, e il committente riceveva ciò che gli veniva dato. È qui che l'AI cambia la geometria: puoi occupare la casella dell'architetto **se sai fare le domande giuste e verificare le risposte**. Questo corso serve esattamente a questo.

**[OPINIONE, dichiarata]** Chi promette che "ora chiunque può costruire software" vende fumo: senza criterio si producono sistemi che funzionano il primo giorno, si sfaldano al terzo mese e non si sa come ripararli. Ma chi dice che "senza saper programmare non tocchi nulla" è ugualmente fuori tempo. La verità operativa è la terza: **un committente competente del proprio dominio, con metodo, ottiene sistemi di valore reale — se conosce i propri limiti.**

### Il paragone che ti sarà utile

Dirigere lo sviluppo assomiglia molto a ciò che già fai con la faculty di un congresso. Non tieni tu le relazioni scientifiche: **decidi il programma, scegli i relatori, imponi i tempi, verifichi che il risultato sia all'altezza e ti prendi la responsabilità del risultato finale.** Non serve essere anestesista per dirigere Romanestesia — serve saper riconoscere una sessione mal costruita. Con Giano è identico: non serve scrivere codice, serve **riconoscere un sistema mal costruito**. Il resto del corso ti insegna a riconoscerlo.

---

## 3. Le tre domande, prima di ogni riga di codice

### 3.1 Che problema risolvo — per chi, e quanto fa male oggi?

Non "cosa vorrei costruire", ma **quale dolore esiste già**. Per Giano il dolore è reale e nominabile: le associazioni scientifiche gestiscono soci, quote e cariche con Excel, mail e memoria delle persone; ogni assemblea è una ricostruzione manuale di chi ha diritto a cosa; ogni rinnovo è un inseguimento.

**Il test:** se non riesci a nominare **una persona reale** che oggi soffre di questo problema, non stai risolvendo un problema — stai avendo un'idea. Le idee sono gratis; i problemi hanno un committente.

### 3.2 Chi lo userà davvero — e cosa succede se non lo usa?

Un sistema di governance ha almeno quattro utenti diversi, con bisogni divergenti: la **segreteria** (ci vive dentro tutti i giorni), il **presidente/consiglio** (ci entra due volte l'anno e deve capirlo subito), il **tesoriere** (guarda solo i soldi), il **socio** (deve poter rinnovare in tre clic o non rinnoverà).

**Il test:** per ognuno, in una riga: *perché aprirebbe Giano invece di fare come ha sempre fatto?* Se per una categoria la risposta non c'è, quella categoria non lo userà — e va bene saperlo prima, non dopo.

### 3.3 Cosa succede se sparisce?

Domanda scomoda e decisiva. Se Giano domani non ci fosse più: l'associazione perde i dati dei soci? Non può fare l'assemblea? O semplicemente torna all'Excel di prima?

**La risposta determina il livello di rigore che devi.** Se la risposta è "torna all'Excel", puoi permetterti di imparare sbagliando. Se la risposta è "perde il registro dei soci", allora **backup, sicurezza e continuità non sono opzionali dal giorno uno** — sono il prodotto.

Per Giano, onestamente, la risposta è la seconda. Prendine atto ora.

---

## 4. Il confine: cosa non farai da solo

Questa è la parte del modulo che ti chiederà di rinunciare a qualcosa. Fallo adesso, per iscritto, mentre sei lucido: la tentazione di superare il confine arriva sempre più tardi, sotto pressione, quando "manca solo quel pezzo".

### 4.1 Le tre aree proibite (e perché)

**Votazioni, deleghe, elezioni.** Il problema non è tecnico, è di **garanzie dimostrabili**: che il voto sia di chi aveva diritto, che non sia stato alterato, che sia segreto dove lo statuto lo impone, che il risultato sia difendibile davanti a un socio che lo contesta. Un'elezione invalidata non è un bug da correggere il giorno dopo: è un danno alla società scientifica cliente e alla vostra reputazione. **Tu la specifichi (regole statutarie, quorum, deleghe massime, tracciabilità); l'implementazione va a chi ha competenza e responsabilità professionale.**

**Pagamenti.** Non tratterai mai dati di carte di credito: ci si appoggia a un fornitore certificato, e Giano registra soltanto l'esito ("quota 2026 pagata il 14/03"). Questa non è una limitazione: **è il modo giusto di farlo**, e lo fanno così anche i prodotti da milioni di utenti.

**Dati personali di terzi, su larga scala.** Nel momento in cui Giano contiene i soci di un'associazione cliente, quei dati **non sono vostri**: li trattate per conto di qualcun altro, con obblighi conseguenti (accordo sul trattamento, misure di sicurezza documentate, gestione delle richieste degli interessati). Serve un consulente privacy prima del primo cliente reale, non dopo.

### 4.2 Ciò che invece puoi legittimamente dirigere

Anagrafica soci e stati di iscrizione · quote e rinnovi (registrazione, scadenze, solleciti) · cariche e mandati · gruppi di studio e sezioni territoriali · documenti istituzionali · reportistica · integrazioni · l'intera esperienza d'uso. **È la maggior parte di Giano** — ed è esattamente la fetta da cui parte il corso.

---

## 5. Prompt operativi (copiali, non ricordarli)

**Prompt A1.1 — Il confronto avversariale sull'idea**
```
Sono la direzione di una società che organizza congressi medici ECM.
Voglio costruire "Giano": una piattaforma gestionale per società
scientifiche e associazioni professionali (soci, quote, rinnovi, cariche,
gruppi di studio, sezioni, assemblee, candidature, deleghe, votazioni,
patrocini, bandi, documenti istituzionali).

Fai l'avvocato del diavolo. Elenca:
1) i cinque motivi più forti per cui questo progetto potrebbe fallire;
2) le tre parti che sto probabilmente sottovalutando in complessità;
3) le domande che un investitore o un cliente esigente mi farebbe e a cui
   oggi non saprei rispondere.
Non essere incoraggiante: sii utile.
```

**Prompt A1.2 — La mappa degli utenti**
```
Per Giano, elenca gli utenti tipo di una società scientifica (segreteria,
presidente, consiglio direttivo, tesoriere, socio, revisore...). Per
ciascuno: cosa fa oggi senza Giano, cosa farebbe con Giano, e in una riga
perché dovrebbe cambiare abitudine. Segnala quelli per cui il beneficio
è debole: sono i punti dove l'adozione fallirà.
```

**Prompt A1.3 — La carta del progetto (il deliverable)**
```
Aiutami a scrivere la "carta del progetto" di Giano, una pagina, con:
- PROBLEMA (chi soffre oggi, e come)
- COSA È Giano (3 righe)
- COSA NON È (i confini: cosa NON farà, per scelta)
- UTENTI e beneficio per ciascuno
- COSA NON COSTRUIRÒ DA SOLO (votazioni/deleghe, pagamenti, trattamento
  dati di terzi) e perché
- COME SAPRÒ CHE STA FUNZIONANDO (2-3 segnali concreti, non "successo")
- COSA SUCCEDE SE SPARISCE
Fammi prima le domande che ti servono, una alla volta.
```

---

## 6. Il deliverable: la carta del progetto

Una pagina. Non di più. La userai in tre momenti:

1. **Quando Claude ti proporrà di aggiungere una funzione** e sarai tentato di dire sì.
2. **Quando un cliente chiederà qualcosa fuori scopo** e dovrai dire no con eleganza.
3. **Nei mesi tre e sei**, quando l'entusiasmo sarà finito e ti chiederai perché lo stai facendo.

### Come verificare di averla fatta bene

- La sezione **"cosa non è"** è lunga almeno quanto **"cosa è"**. Se non lo è, non hai ancora deciso nulla: hai solo sognato.
- I **segnali di funzionamento** sono osservabili da un estraneo ("l'associazione pilota ha caricato i suoi 400 soci e ha smesso di usare l'Excel"), non stati d'animo ("i clienti sono contenti").
- Il **confine** è scritto in modo che tu stesso, tra sei mesi e sotto pressione, non possa fingere di non averlo capito.

---

## 7. Errori tipici di questa fase

1. **Innamorarsi della piattaforma completa.** Giano nella sua interezza è il prodotto di anni. Chi parte dal tutto non arriva a niente: si parte da una fetta (→ ARCH-02 §3).
2. **Chiamare "prodotto" ciò che non ha un cliente.** Un pilota reale, anche gratuito, vale più di dieci funzionalità.
3. **Rimandare il confine.** "Le votazioni le vediamo dopo" è il modo in cui si finisce a implementarle di corsa, male, la settimana prima di un'assemblea.
4. **Confondere la propria competenza di dominio con competenza tecnica.** Sai come funziona la governance associativa meglio di qualsiasi sviluppatore — e questo è il tuo vantaggio enorme. Non è la stessa cosa che saper costruire un sistema sicuro.
5. **Usare Claude come sostegno morale.** Se gli chiedi "bella idea, vero?", ti dirà di sì (→ F1 §4.4). Per questo il primo prompt del modulo è avversariale.

---

## 8. Prima del modulo A2

Eseguito A1, mi servono quattro informazioni per proseguire (sono in ARCH-02 §6):
**Aurora, Madeira e LMS** cosa sono e a che punto sono · se **esiste già qualcosa** di Giano (documenti, prototipi, preventivi ricevuti) · se lavori **da solo** o c'è già un fornitore tecnico · se hai in vista una **associazione pilota** reale.

---

*Changelog: v0.1 — prima stesura; adattato al progetto Giano dopo la scelta della direzione del 13/07/2026.*
