# A6 — Scelte di piattaforma: costruire, comprare, o evitare

**Corso 3 · Parte B · v1.0**

Le scelte di piattaforma sono **decisioni da architetto, non da esecutore**: hanno conseguenze economiche, strategiche e di rischio che il codice non vede. Un modello ti dirà volentieri quale libreria usare; non sa quanto ti costerà uscirne fra tre anni, né cosa succede alla tua azienda se il fornitore chiude.

---

## 1. Il criterio maestro: la differenziazione

> **Costruisci ciò che ti differenzia. Compra tutto il resto. Evita ciò che non serve.**

Un'autenticazione fatta in casa non ti dà **alcun** vantaggio competitivo: ti dà superficie d'attacco, manutenzione, e un vettore di incidenti gravi. La conoscenza del dominio — come funziona davvero una segreteria ECM, quali sono le eccezioni di uno statuto, perché le quote si incassano in quel modo — **quella** è il tuo vantaggio, e lì vale la pena costruire.

**La domanda operativa,** da fare su ogni componente:

> *Se questa cosa la facesse un fornitore, in modo standard, perderei qualcosa che i miei clienti pagano?*

Se la risposta è no → compra. Ogni ora spesa a ricostruire un pezzo di commodity è un'ora sottratta all'unico posto dove il tuo vantaggio è difendibile.

**Il rovescio, altrettanto importante:** non comprare ciò che *è* il tuo prodotto. Se esternalizzi la logica che rappresenta il tuo dominio, hai costruito un rivenditore, non un prodotto.

---

## 2. Le sei domande su ogni fornitore

Non "è buono?", ma:

### 2.1 Quanto costa alla scala prevista, non a quella di oggi
I prezzi cloud crescono **a scalini**, non linearmente: c'è un piano gratuito generoso, poi un salto. **Dove sono gli scalini?** Modella il costo a 10, 100, 1.000 utenti *prima* di scegliere. Il fallimento tipico non è "costa troppo": è "costava 0 € e improvvisamente costa 400 €/mese, e non possiamo più andarcene".

### 2.2 Quanto costa uscirne
**Il costo di uscita è il vero prezzo del fornitore**, perché determina il tuo potere negoziale futuro. Le domande concrete:
- I dati me li porto via in un formato standard, o proprietario?
- Il codice che ho scritto funziona altrove, o è pieno di API loro?
- Quanto tempo mi servirebbe, realisticamente, per migrare?

Un fornitore che rende difficile uscire **ha già vinto ogni trattativa futura sui prezzi**. Non è malvagio: è il suo modello di business, ed è legittimo. Il tuo lavoro è saperlo mentre firmi, non dopo.

### 2.3 Cosa succede se sparisce
Acquisizione, cambio di licenza, chiusura, cambio di prezzi ostile: succede regolarmente, e ai fornitori grandi quanto ai piccoli. **La domanda non è "succederà?" ma "quanto mi costerebbe?"**

### 2.4 Chi risponde quando si rompe alle 3 di notte
E con quali garanzie contrattuali (SLA). Un servizio gratuito non ha nessuno che risponde — il che è perfettamente accettabile finché non ci costruisci sopra il tuo prodotto.

### 2.5 Il fornitore è allineato con il tuo caso d'uso?
Sei un cliente marginale per loro, o sei nel loro segmento? Un cliente marginale subisce le decisioni prese per altri: cambi di prodotto, deprecazioni, prezzi ripensati.

### 2.6 Quanto è profondamente intrecciato al tuo sistema?
Un fornitore che tocca **un** punto del sistema si sostituisce. Uno che ne tocca cinquanta, no. Questo dipende da te, non da lui → §4.

---

## 3. La matrice della decisione

| | **Costruisci** | **Compra** | **Non fare** |
|---|---|---|---|
| **Quando** | È il tuo dominio, il tuo vantaggio, il tuo differenziale | È risolto, standard, e sbagliarlo fa male (auth, pagamenti, email, cifratura) | Nessuno l'ha chiesto davvero; risolve un problema che puoi tollerare |
| **Rischio** | Tempo, manutenzione eterna | Lock-in, costi, dipendenza | Nessuno |
| **Esempi tipici** | Logica di dominio, flussi, regole | Autenticazione, pagamenti, invio email, hosting, database, monitoraggio | La dashboard "che sarebbe bello avere" |

**La terza colonna è quella che nessuno considera, ed è spesso la risposta giusta.** Ora che costruire costa poco, la domanda "possiamo costruirlo?" è diventata inutile: la risposta è quasi sempre sì. **La domanda utile è: "vale la pena che questa cosa esista, per i prossimi cinque anni, dati tutti i suoi costi?"** (→ A16 per il calcolo completo.)

---

## 4. Isolare il fornitore: l'unica difesa strutturale

Non puoi evitare il lock-in. **Puoi decidere quanto è profondo.** La tecnica ha un nome (*anti-corruption layer*, o *ports and adapters*) e in linguaggio da committente è semplicissima:

> **Il resto del tuo sistema non deve sapere quale fornitore stai usando.**

In pratica: tutto il codice che parla con un fornitore vive in un solo posto, dietro un'interfaccia scritta nei termini del *tuo* dominio.

❌ Il sistema chiama direttamente `stripe.charges.create(...)` in dodici punti diversi. Cambiare fornitore = riscrivere dodici punti, con la certezza di dimenticarne uno.

✅ Il sistema chiama `pagamenti.incassaQuota(socio, importo)`. Dentro, quella funzione parla con il fornitore. Cambiare fornitore = riscrivere **una** funzione.

**Il costo di questa disciplina è quasi zero se la applichi dall'inizio, ed è enorme se la aggiungi dopo.** È una delle pochissime cose che vale la pena fare "in anticipo", perché il costo è asimmetrico.

**Il prompt che la impone:**

```
Tutto il codice che comunica con [FORNITORE] deve stare in un unico modulo,
dietro un'interfaccia espressa nei termini del nostro dominio (non nei loro).
Nessun altro punto del sistema deve importare il loro SDK o conoscere i loro
tipi. Mostrami l'interfaccia prima di implementarla.
```

**Ma non farlo per tutto.** Isolare il *database* dietro un'astrazione generica è quasi sempre uno spreco: cambiare database è un evento raro, e l'astrazione ti costa espressività ogni giorno. **Isola ciò che è plausibile sostituire** (pagamenti, email, storage, servizi AI), non ciò che sostituirai forse mai.

---

## 5. Il debito da comodità

Le piattaforme che rendono *facilissimo* iniziare rendono spesso *difficilissimo* andarsene. È il loro modello, ed è dichiarato: ti regalano i primi mesi perché sanno che poi non potrai più uscire.

La contromossa non è l'ascetismo tecnologico (ricostruire tutto da soli è il modo migliore di non finire mai). È:

1. **Usa la comodità dove il lock-in è poco profondo** (hosting: si cambia in un giorno).
2. **Isola dove è profondo** (autenticazione: se gli utenti vivono nel loro sistema, migrarli è un progetto).
3. **Tieni i dati portabili, sempre.** Se i tuoi dati sono in un database standard (Postgres) e ne hai un export funzionante, hai sempre una via d'uscita — indipendentemente da tutto il resto.

> **La portabilità dei dati è l'ultima linea di difesa, e va difesa a ogni costo.** Puoi permetterti di essere legato a un fornitore di hosting, di email, persino di autenticazione. Non puoi permetterti di essere legato a chi tiene i tuoi dati in un formato che solo lui capisce.

---

## 6. Un giudizio, dichiarato

**[OPINIONE, con motivazione]** Per un prodotto verticale gestito da una struttura piccola, la combinazione che riduce di più il rischio è: **database relazionale standard** (Postgres — così i dati restano portabili qualunque cosa accada), **hosting sostituibile in un giorno**, **fornitori esterni per autenticazione, pagamenti ed email**, e **tutta la logica di dominio scritta da te, isolata da tutto il resto**.

Non perché sia la scelta più elegante o la più moderna, ma perché è quella in cui **ogni singolo pezzo può essere sostituito senza uccidere il prodotto** — che è l'unica proprietà che conta davvero quando il progetto vive più a lungo delle mode tecnologiche che lo hanno visto nascere.

---

## Strumenti di questo modulo

1. **Il criterio della differenziazione** — costruisci ciò che ti differenzia, compra il resto, evita il superfluo.
2. **Le sei domande su ogni fornitore**, incluse quelle sul costo di uscita.
3. **La matrice costruisci/compra/non fare**, con la terza colonna sempre sul tavolo.
4. **L'isolamento del fornitore** (anti-corruption layer) e il prompt che lo impone.
5. **La regola della portabilità dei dati** come ultima linea di difesa.

---

## Errori tipici

1. **Scegliere sul prezzo di oggi** e trovarsi lo scalino a scala 10x.
2. **Ricostruire commodity** (autenticazione, email) per orgoglio o per "risparmio".
3. **Comprare il proprio dominio**, cioè esternalizzare l'unica cosa che ti differenzia.
4. **Spargere lo SDK del fornitore ovunque**, rendendo la sostituzione una riscrittura.
5. **Astrarre tutto**, pagando ogni giorno per una flessibilità che non userai mai.
6. **Dimenticare la terza colonna:** la cosa migliore che puoi costruire, spesso, è niente.

*v1.0 — riscrittura in profondità.*
