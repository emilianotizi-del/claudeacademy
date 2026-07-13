# A12 — Produzione: quando qualcuno dipende da te

**Corso 3 · Parte C · v1.0**

La produzione non è uno stadio di maturità del codice: è uno **stato di obbligo**. Dal momento in cui una persona reale ci lavora dentro, hai contratto un debito verso di lei — e i tuoi errori sono diventati i suoi problemi.

---

## 1. Cosa cambia, riga per riga

| | Prototipo | Produzione |
|---|---|---|
| I dati | Si buttano | **Sono la verità di qualcuno** |
| Il fermo | Irrilevante | È un danno, e va comunicato |
| Il rilascio | Quando ti va | Con procedura, reversibile |
| I bug | Li scopri usando | Li scoprono i clienti — **se non hai osservabilità** |
| Il codice brutto | Un fastidio tuo | Un debito che paghi ogni mese |
| Un errore | Si corregge | Può essere una violazione di dati da notificare in 72 ore |

**La transizione non è tecnica: è di responsabilità.** E il momento in cui avviene non è il primo deploy: è **il primo dato reale inserito da qualcuno che non sei tu**.

---

## 2. La migrazione dei dati: il momento più pericoloso dell'intero progetto

Qui muoiono i progetti seri. L'importazione dell'Excel dei soci sembra un compito banale — è invece il punto in cui si perdono dati **in modo irreversibile e silenzioso**.

### Perché i dati reali sono sempre peggio del previsto
Nomi con accenti e apostrofi. Date in tre formati diversi nella stessa colonna. Email in maiuscolo, con spazi, o due nella stessa cella. Duplicati che non sono duplicati ("Mario Rossi" a Roma e a Milano sono due persone; "Rossi Mario" e "Mario Rossi" sono la stessa). Campi vuoti che significano cose diverse ("non lo sappiamo" e "zero" sono entrambi vuoti). Righe di totali in mezzo ai dati. Colonne con note libere che contengono informazioni essenziali che nessuno ha mai formalizzato.

**Nessuna di queste cose è nel tuo modello dati.** E il codice generato assume dati puliti, perché il tuo esempio lo era.

### Il protocollo di migrazione, in otto passi

```
1. COPIA. L'originale non si tocca mai. Mai. Si lavora su copia.
2. PROFILA prima di importare:
   - quante righe? quanti valori distinti per colonna?
   - quante celle vuote, per colonna?
   - quali formati compaiono nelle date? nei numeri?
   - quanti potenziali duplicati, e con quale criterio?
   Questo passo scopre il 90% delle sorprese, e costa dieci minuti.
3. DEFINISCI LE REGOLE, per iscritto, con una persona che conosce i dati:
   cosa è un duplicato, cosa fare con i campi vuoti, come normalizzare.
   NON decidere queste cose da solo, e NON lasciarle decidere al modello.
4. IMPORTA IN UN AMBIENTE DI PROVA, mai direttamente in produzione.
5. CONTA PRIMA E DOPO. Righe in ingresso, record creati, righe scartate.
   La somma DEVE tornare. Ogni riga scartata ha un motivo, scritto.
6. FAI VALIDARE IL RISULTATO a chi conosce i dati — non a chi ha scritto
   la migrazione, e non a te.
7. PROVA IL RITORNO INDIETRO. Se l'importazione va male, come si annulla?
8. SOLO ALLORA, in produzione. Con backup fresco e provato.
```

**Il passo 5 è quello che nessuno fa e che salva tutto.** "Abbiamo importato 412 righe e creati 398 record" — *dove sono finite le altre 14?* Se non sai rispondere, hai perso dati e non lo sai. **Il silenzio è il modo in cui i dati muoiono.**

---

## 3. Il deploy

### Il risultato che devi conoscere
Il programma di ricerca **DORA** (decine di migliaia di team, anni di dati) ha stabilito un fatto controintuitivo e solidissimo: i team che rilasciano **piccolo e spesso** hanno **contemporaneamente** più velocità **e** meno incidenti dei team che rilasciano grande e raro.

Non è un compromesso tra velocità e sicurezza: **le due cose vanno insieme**, e il meccanismo è ovvio una volta detto — un cambiamento piccolo è comprensibile, verificabile, e soprattutto **annullabile**. Quando un rilascio contiene una cosa e si rompe, sai cosa annullare. Quando ne contiene quaranta, comincia l'archeologia, di solito di notte.

**Corollario contro l'istinto:** il grande rilascio "attentamente pianificato" del venerdì sera è la pratica più pericolosa che esista, ed è quella che *sembra* più prudente.

### Le tre proprietà di un deploy sano
1. **Automatico.** Se il deploy è un rituale manuale di 12 passi, un giorno qualcuno ne salterà uno. Sarà il giorno peggiore.
2. **Reversibile in un comando** — e **provato**, non presunto. La domanda giusta non è "funziona il deploy?" ma *"quanto ci metto a tornare alla versione di prima?"*
3. **Osservato.** Dopo ogni rilascio, si guardano gli errori per dieci minuti. Rilasciare e chiudere il portatile è la scommessa più cara che puoi fare.

### Le migrazioni di schema in produzione
**Il codice si può annullare. Una migrazione che ha già cancellato una colonna, no.** Da qui la regola:

> **Le migrazioni sono additive prima, distruttive poi — e in due rilasci separati.**

Il pattern in quattro passi, che elimina il downtime e i disastri:
1. **Aggiungi** la nuova colonna (il vecchio codice la ignora, non si rompe nulla).
2. **Scrivi in entrambe** (nuovo codice, doppia scrittura).
3. **Migra** i dati vecchi, con calma, verificando.
4. **Solo dopo**, in un rilascio successivo, **rimuovi** la vecchia.

Quattro rilasci invece di uno. È il prezzo della reversibilità, ed è basso.

---

## 4. La checklist di lancio

```
DATI
□ Backup fatto oggi, e ripristino PROVATO (non presunto)
□ Migrazione provata su copia, con conteggi che tornano
□ Piano di ritorno indietro scritto e provato

ACCESSI
□ So chi ha accesso a cosa, e perché
□ Le credenziali di produzione non sono sulla mia macchina di sviluppo
□ Nessun segreto nel codice o nei log

SICUREZZA
□ Test di isolamento fra clienti superato (→ A9)
□ Audit avversariale eseguito sui percorsi che toccano dati e permessi

OSSERVABILITÀ
□ Gli errori mi arrivano — non aspetto che sia il cliente a dirmelo
□ I log permettono di ricostruire chi ha fatto cosa
□ Ho un allarme su almeno un'invariante di dominio

PERSONE
□ Qualcuno oltre a me sa cosa fare se si rompe
□ L'utente sa a chi scrivere, e riceve risposta
□ So cosa dire, e a chi, se perdo dei dati
```

**L'ultima riga della sezione "persone" è quella che nessuno prepara e che decide come finisce.** Un incidente gestito bene può *rafforzare* la fiducia di un cliente; lo stesso incidente gestito con il silenzio la distrugge.

---

## 5. L'incidente: cosa fare, in ordine

1. **Ferma l'emorragia.** Prima il ripristino del servizio, poi la comprensione. Se il ritorno alla versione precedente risolve, si torna indietro *e poi* si indaga. **Non si debugga in produzione mentre gli utenti sanguinano.**
2. **Comunica presto, anche se non sai ancora tutto.** "Stiamo indagando su un problema, vi aggiorniamo entro un'ora" è infinitamente meglio del silenzio. Il silenzio viene interpretato come incompetenza o malafede.
3. **Preserva le prove.** Log, stato del database, orari. Non ripulire prima di aver capito.
4. **Poi indaga** (→ A10).
5. **Post-mortem senza colpevoli.** Non "chi è stato" ma **"cosa ha reso possibile questo errore?"** — un sistema in cui un errore umano causa un disastro è un sistema mal progettato, indipendentemente da chi ha premuto il tasto. L'output del post-mortem è **un cambiamento strutturale**, non una promessa di stare più attenti (le promesse non sopravvivono al primo venerdì di fretta).
6. **Se ci sono dati personali di mezzo:** valuta l'obbligo di notifica (72 ore sotto GDPR, all'autorità; e agli interessati, se il rischio è elevato). **Questa valutazione non si improvvisa il giorno stesso**: si decide prima con chi di dovere.

---

## Strumenti di questo modulo

1. **Il protocollo di migrazione in 8 passi**, con conteggi che devono tornare.
2. **Il pattern additivo-poi-distruttivo** per le migrazioni di schema.
3. **Le tre proprietà del deploy sano**: automatico, reversibile provato, osservato.
4. **La checklist di lancio**, incluse le righe sulle persone.
5. **La procedura d'incidente in 6 passi**, con post-mortem senza colpevoli.

---

## Errori tipici

1. **Importare senza contare.** I dati spariscono in silenzio.
2. **Far validare la migrazione a chi l'ha scritta.**
3. **Rilasci grandi e rari**, chiamati prudenza.
4. **Migrazioni distruttive in un colpo solo.**
5. **Nessun piano di comunicazione** per il giorno in cui va storto.
6. **Post-mortem che cerca colpevoli.** Garantisce che il prossimo errore venga nascosto invece che segnalato — e i problemi nascosti crescono.

*v1.0 — riscrittura in profondità.*
