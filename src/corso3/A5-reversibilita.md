# A5 — Reversibilità: la proprietà che ti permette di osare

**Corso 3 · Parte B · v1.0**

Il principio non è tecnico, è strategico:

> **La velocità sostenibile è una funzione della reversibilità.** Se ogni azione è irreversibile, ogni azione richiede cautela, e tutto rallenta. Se puoi tornare indietro in trenta secondi, puoi permetterti di sbagliare in fretta — che è il modo più rapido di imparare.

Con un esecutore che produce molto e sbaglia in modi non ovvi, la rete di sicurezza non è un'igiene: **è ciò che rende praticabile l'intero metodo.**

---

## 1. Git per committenti: i cinque concetti che bastano

Non ti serve padroneggiare git. Ti servono cinque idee e la capacità di chiedere le operazioni giuste.

1. **Commit = punto di ripristino.** Ogni commit è uno stato a cui puoi tornare esattamente. Un commit dovrebbe essere **una cosa sola, funzionante**: se contiene tre cose, non puoi annullarne una.
2. **Branch = universo parallelo.** Lavori su un ramo, la versione buona resta intatta. Costa zero e ti rende impavido.
3. **Diff = cosa è cambiato.** È l'unità che leggi tu (→ A8). Non leggi il sistema: leggi il delta.
4. **Storia = il perché.** I messaggi di commit sono documentazione. `fix` non è un messaggio; `fix: le iscrizioni duplicate quando il doppio click arriva prima della risposta` è un messaggio.
5. **Revert = annullare senza cancellare.** Torni indietro creando un nuovo commit che disfa: la storia resta, e puoi cambiare idea di nuovo.

**La disciplina minima:** un commit per unità logica, messaggio che dice *perché* (il *cosa* si vede dal diff), e mai commit su un sistema che non passa i test.

### Il prompt che rende git indolore

```
Ho fatto un pasticcio: [descrizione]. Prima di eseguire qualsiasi comando
git, dimmi: 1) cosa è successo, 2) le opzioni per rimediare con i rispettivi
rischi, 3) cosa perderei con ciascuna. Non eseguire nulla finché non scelgo.
```

Regola d'oro: **prima di ogni operazione git distruttiva** (`reset --hard`, `force push`, `rebase`), fai una copia del ramo (`git branch backup-$(date +%s)`). Costa un secondo e ti salva la giornata.

---

## 2. Ambienti: dove si sperimenta e dove si fa male

Tre ambienti, tre significati diversi. È la separazione più sottovalutata da chi costruisce da solo.

| Ambiente | Dati | Chi lo usa | Regola |
|---|---|---|---|
| **Locale** | Finti, generati | Tu, per costruire | Distruggilo pure ogni giorno |
| **Staging** | Copia anonimizzata o finti realistici | Tu + tester, per provare | Deve somigliare alla produzione, o non serve a niente |
| **Produzione** | **Veri, di persone vere** | Gli utenti | Non si tocca a mano. Mai. |

**La regola che devi tatuarti:** *se ti trovi a eseguire un comando manuale sul database di produzione, sei già in una situazione che andava evitata.* Le modifiche in produzione avvengono tramite codice versionato e migrazioni testate, non tramite un umano stanco alle 23:00.

**Il caso concreto che capita a tutti.** Sei convinto di essere sul database di prova, esegui una cancellazione, e non lo eri. Contromisure, in ordine di efficacia:
1. Credenziali di produzione **non presenti** sulla tua macchina di sviluppo.
2. Prompt del terminale che urla in rosso quando sei su produzione.
3. Accesso in sola lettura per default; la scrittura richiede un passaggio deliberato.

---

## 3. Backup: la differenza tra averlo e averlo provato

> **Un backup mai ripristinato non è un backup: è una speranza.**

Il fallimento classico non è "non avevamo il backup". È: *avevamo il backup, ma il ripristino non funzionava* (formato, permessi, chiavi mancanti), oppure *ci sono volute 14 ore* (e il congresso era il giorno dopo), oppure *conteneva già i dati corrotti* perché la corruzione era iniziata tre settimane prima.

**Le tre domande del backup:**
1. **RPO** — quanti dati posso permettermi di perdere? (un'ora? un giorno?) Determina la frequenza.
2. **RTO** — quanto tempo posso stare fermo? Determina il tipo di ripristino.
3. **Retention** — quanto indietro posso andare? Determina se sopravvivi a una corruzione lenta o a un ransomware.

**La prova di ripristino** va fatta **una volta**, davvero, cronometro alla mano, e ripetuta a ogni cambio strutturale. È noiosa, è l'unica cosa che trasforma la speranza in una garanzia, e il 90% dei progetti non la fa mai.

---

## 4. I segreti

Chiavi, token, password. Le regole sono poche e non negoziabili.

1. **Non stanno nel codice.** Mai, nemmeno "temporaneamente". La storia di git è per sempre: un segreto committato è bruciato anche se lo togli al commit dopo — resta nella storia, ed esistono robot che scandagliano GitHub *in continuazione* cercandoli. Il tempo medio tra la pubblicazione di una chiave AWS su GitHub e il suo primo utilizzo abusivo si misura in **minuti**.
2. **Non stanno nelle chat.** (Nota per te, che è il motivo per cui te l'ho ripetuto: il token usato in questo percorso è transitato in una conversazione. È da considerarsi esposto: si revoca e si rigenera, non si spera.)
3. **Vivono in variabili d'ambiente o in un gestore di segreti**, con permessi minimi: un token che può solo leggere un repo non può cancellartelo.
4. **Si ruotano periodicamente**, e immediatamente quando escono dal loro perimetro.
5. **Segreti diversi per ambienti diversi.** Il token di sviluppo non deve poter toccare la produzione. Sembra ovvio; è la causa di metà degli incidenti reali.

**Cosa fare se hai committato un segreto:** revoca *prima* (subito, dalla dashboard del servizio), pulisci la storia *dopo* (e sappi che potrebbe essere già stato clonato). L'ordine è questo, e non è negoziabile: la pulizia della storia senza revoca è teatro.

---

## 5. Il deploy come atto reversibile

**Il risultato empirico più solido dell'ingegneria del software degli ultimi vent'anni** (documentato dal programma di ricerca DORA, su decine di migliaia di team): i team che rilasciano **piccolo e spesso** hanno *contemporaneamente* più velocità **e** meno incidenti dei team che rilasciano grande e raro. Non è un compromesso: le due cose vanno insieme.

Il motivo è meccanico, non culturale: la dimensione del cambiamento predice il rischio meglio di qualunque altra variabile, perché un cambiamento piccolo è comprensibile, verificabile, e **annullabile**. Quando un rilascio contiene una cosa e si rompe, sai cosa annullare. Quando ne contiene quaranta, inizia l'archeologia — di solito di notte.

**Le tre proprietà di un deploy sano:**
- **Piccolo** — una cosa alla volta.
- **Automatico** — se il deploy è un rituale manuale di 12 passi, un giorno qualcuno ne salterà uno; e sarà venerdì.
- **Annullabile in un comando** — e devi averlo *provato*, non presunto.

---

## 6. La checklist di reversibilità

Da rileggere prima di ogni azione rischiosa. Se una risposta è "non lo so", fermati: **non lo sai è la risposta più costosa del software.**

```
□ Se questa modifica è un disastro, come torno indietro? In quanto tempo?
□ L'ho già provato, o lo sto presumendo?
□ Sto lavorando su dati veri? Perché?
□ Se il ripristino fallisse, quanti dati perderei?
□ Qualcuno se ne accorgerebbe, se andasse storto in silenzio?
□ Il segreto che sto usando può fare più danni del necessario?
```

---

## Strumenti di questo modulo

1. **I cinque concetti di git** e il prompt "non eseguire finché non scelgo".
2. **La separazione dei tre ambienti**, con le contromisure contro il comando sul database sbagliato.
3. **Le tre domande del backup** (RPO/RTO/retention) e la prova di ripristino cronometrata.
4. **Le cinque regole dei segreti**, con l'ordine giusto in caso di esposizione: revoca, poi pulisci.
5. **La checklist di reversibilità.**

---

## Errori tipici

1. **Commit enormi**: ti tolgono l'unica cosa che li rendeva utili, cioè poter annullare *un pezzo*.
2. **Backup mai ripristinato.**
3. **Un solo ambiente**, con i dati veri, "tanto sto attento".
4. **Segreto committato e "tolto"**, senza revoca. È come cambiare la serratura dopo aver dato la chiave in fotocopia.
5. **Deploy grande e raro**, chiamato "prudenza". È l'opposto.

*v1.0 — riscrittura in profondità.*
