# ARCH-02 — Corso 3 · AI Software Architect — architettura proposta

**Codice:** ARCH-02 · **Versione:** 0.1 (proposta, da approvare) · 13/07/2026
**Destinatario unico:** direzione (Emiliano Tizi) · **Prerequisito:** Corso 1 assorbito (F1–F6)
**Dipendenze:** DATI-01 v1.1 · non duplica F1–F6 e M1–M22: li presuppone

---

## 1. La tesi del corso

Gli altri due prodotti insegnano a **usare** Claude. Questo insegna a **dirigerlo** su un terreno dove non hai competenza tecnica di partenza: la costruzione di software.

> **Non diventerai un programmatore. Diventerai il committente tecnico che sa cosa chiedere, come verificarlo e quando fermarsi.** È esattamente il mestiere che già fai con la faculty e con i fornitori — applicato al codice.

**[OPINIONE — dichiarata]** Questa è la scommessa del percorso, ed è dibattuta. Chi dice che "chiunque può programmare con l'AI" sbaglia: senza criterio si producono sistemi che funzionano il primo giorno e diventano ingestibili il terzo mese. Chi dice che "senza saper programmare non si può fare nulla" sbaglia altrettanto: con metodo, un committente esperto del proprio dominio ottiene strumenti interni di valore reale. Il corso sta nel mezzo e insegna i confini: **cosa puoi fare da solo, cosa puoi fare con Claude, quando devi chiamare un professionista.**

---

## 2. Il principio didattico: un solo progetto vero

Quindici moduli teorici non ti servirebbero a nulla. Il corso è costruito attorno a **un progetto reale che porti in produzione**, e ogni modulo è una tappa di quel progetto. Alla fine avrai: lo strumento funzionante, il metodo, e la capacità di ripetere il ciclo su altri strumenti.

### Il progetto candidato (da confermare — vedi §7)

**"Sponsor Tracker" — cruscotto interno del ciclo sponsor.**
Perché è il candidato giusto:
- **Dolore reale documentato:** dal verbale della riunione emergono sei trattative parallele (Nutricia, Baxter, Fresenius, GE, Viatris, Edwards) inseguite via messaggi e memoria — con deliverable e scadenze sparsi.
- **Dimensione onesta:** costruibile in poche settimane, non un gestionale.
- **Basso rischio:** dati aziendali interni, non dati sanitari; nessuna sostituzione di Fattureincloud o Hera Education (M14: i gestionali restano la fonte).
- **Valore immediato:** ti dice, ogni lunedì, chi è fermo, chi deve cosa, quali deliverable mancano prima della stampa.
- **Copre tutti i temi tecnici:** dati (Supabase), interfaccia (Vercel), autenticazione, ruoli, deploy, manutenzione.

**Alternative** (se preferisci): *Radar faculty* (stato inviti, viaggi, rimborsi) · *Cruscotto evento* (checklist e scadenze trasversali) · *Motore di libreria prompt* (interno al team).

---

## 3. Struttura: cinque parti, quindici moduli

### PARTE A — Il mestiere del committente tecnico (A1–A3)
*Cosa cambia quando l'esecutore è un'AI e il committente sei tu*

- **A1 · Cosa vuol dire "dirigere" lo sviluppo.** Il triangolo committente/architetto/esecutore e chi lo occupa ora. Le tre domande da fare prima di scrivere una riga: che problema risolvo, chi lo userà, cosa succede se sparisce. Il confine invalicabile: dove il fai-da-te diventa irresponsabile (dati sanitari, pagamenti, sistemi critici) → richiama F6.
- **A2 · Il vocabolario minimo indispensabile.** Trenta termini spiegati per analogia con il tuo mondo: frontend/backend, database, API, repository, commit, branch, deploy, ambiente, autenticazione, migrazione. Non per programmare: **per non essere raggirabile** e per scrivere richieste che Claude capisca.
- **A3 · Specificare invece di programmare.** Come si scrive una specifica che Claude possa eseguire: requisiti, casi d'uso, criteri di accettazione ("è fatto quando…"), cosa è esplicitamente fuori scopo. È F2 portato al livello di sistema.

### PARTE B — Gli strumenti del mestiere (A4–A6)
*Il tuo banco di lavoro, montato una volta e usato per sempre*

- **A4 · Claude Code: l'esecutore.** Che cos'è, come si installa, come si lavora "a sessioni". La differenza tra chiedere codice in chat (ti dà testo) e Claude Code (agisce sui file, esegue, corregge). Il file di istruzioni permanente del progetto — l'equivalente delle istruzioni di Project (→ F4).
- **A5 · GitHub: la memoria del progetto.** Repository, commit, cronologia, ripristino. **Il concetto che salva la vita:** ogni stato funzionante è recuperabile, quindi puoi permetterti di sbagliare. Sicurezza dei segreti (token, chiavi) — con il caso concreto del token di questo percorso.
- **A6 · Vercel e Supabase: dove la cosa vive.** Hosting e database in parole tue: cosa fanno, quanto costano, cosa succede se un giorno smetti di pagare. Ambienti separati (prova/produzione): la regola che impedisce di distruggere i dati veri mentre sperimenti.

### PARTE C — Costruire (A7–A11)
*Il progetto, dalla prima riga al primo utente*

- **A7 · Dalla specifica al prototipo in un pomeriggio.** Il primo ciclo completo: specifica → prompt → prototipo che gira → lo guardi → correggi. Deliverable: **prototipo dello Sponsor Tracker visibile in un browser.**
- **A8 · I dati: modellare prima di costruire.** Come si disegna una tabella (sponsor, trattative, deliverable, scadenze) senza sapere SQL — e perché mezz'ora spesa qui salva settimane. Deliverable: **il modello dati approvato.**
- **A9 · L'interfaccia: farla usare a qualcun altro.** Le cinque regole di un'interfaccia interna che i colleghi useranno davvero. Il test spietato: dai lo strumento a una persona del team senza spiegazioni e guarda in silenzio. Deliverable: **prima versione usabile.**
- **A10 · Verificare il codice che non hai scritto.** Il modulo più importante del corso. Cinque tecniche: la revisione avversariale ("cerca i modi in cui questo si rompe"), i test automatici spiegati al committente, il controllo dei dati sensibili, la lettura del diff, la seconda opinione (una chat pulita che critica il lavoro dell'altra). L'antidoto all'illusione: *funziona* ≠ *è corretto*.
- **A11 · Mettere in produzione.** Il deploy, il primo utente vero, i primi tre bug. La checklist di lancio: chi ha accesso, dove sono i dati, cosa fare se si rompe di venerdì sera.

### PARTE D — Governare (A12–A14)
*Ciò che separa uno strumento vivo da un giocattolo abbandonato*

- **A12 · Manutenzione e debito tecnico.** Perché il codice "invecchia" anche se non lo tocchi. Il ritmo minimo sostenibile (un'ora al mese). Quando riscrivere e quando lasciar morire uno strumento: **la dismissione è una decisione legittima**, non un fallimento.
- **A13 · Sicurezza e dati, sul serio.** Il livello di rigore cambia quando i dati non sono in una chat ma in un database che vive online: accessi, backup, segreti, cosa NON deve mai finirci (→ F6 con l'asticella alzata). Quando serve un professionista: il confine, scritto nero su bianco.
- **A14 · Costi e dipendenze.** Quanto costa davvero (servizi, tempo tuo, manutenzione). Il rischio di dipendenza da un fornitore e come limitarlo. La domanda che pochi si fanno: **e se tra due anni tu non ci fossi? chi lo mantiene?** — dalla documentazione all'exit plan.

### PARTE E — Ripetere (A15)
- **A15 · Il metodo, generalizzato.** Il ciclo completo estratto dal progetto e reso ripetibile: dal problema all'idea, dalla specifica al prototipo, dal prototipo alla produzione, dalla produzione alla manutenzione. **Il tuo portafoglio di progetti futuri:** come valutare cosa merita di essere costruito e cosa no — con una griglia di 8 domande. Chiusura: cosa delegare, cosa comprare, cosa costruire.

---

## 4. Cosa avrai in mano alla fine

1. Lo **Sponsor Tracker in produzione**, usato dal team.
2. Il **metodo** documentato, per costruire il prossimo strumento senza ripartire da zero.
3. La **capacità di committenza**: saper valutare un preventivo di sviluppo, dialogare con un fornitore tecnico, riconoscere una promessa impossibile.
4. Un **confine chiaro** su ciò che non va fatto da soli.

---

## 5. Formato dei materiali

Diverso dagli altri corsi, perché l'uso è diverso:

- **Sorgenti md** → nel repo, come sempre.
- **Sito**: una sezione dedicata (`architect.html`), con i moduli e — novità — i **prompt operativi pronti** per ogni fase (sono lunghi: vanno copiati, non ricordati).
- **Niente slide**: non è un corso d'aula, è un percorso di lavoro. Al loro posto, per ogni modulo: *obiettivo · concetti · prompt pronti · deliverable · come verificare di averlo fatto bene · errori tipici*.
- **Un diario di bordo** (`DIARIO-01.md`): tieni traccia di cosa hai costruito, cosa si è rotto, cosa hai imparato. È anche il documento che permette a un tecnico esterno di raccapezzarsi domani.

---

## 6. Steps di sviluppo proposti (ordine di produzione)

| Step | Cosa produco | Perché in quest'ordine |
|---|---|---|
| **1** | **A1–A3** (il mestiere: dirigere, vocabolario, specificare) | È la parte che regge tutto: senza il vocabolario e la specifica, gli strumenti sono inutili |
| **2** | **A4–A6** (Claude Code, GitHub, Vercel/Supabase) + setup guidato del tuo banco di lavoro | Da qui in poi lavori davvero: ti serve l'ambiente pronto |
| **3** | **La specifica dello Sponsor Tracker**, scritta insieme applicando A3 | È il documento che guiderà tutta la Parte C |
| **4** | **A7–A9** (prototipo, dati, interfaccia) — modulo + costruzione reale | Ogni modulo produce un pezzo funzionante |
| **5** | **A10–A11** (verifica e produzione) | La verifica prima del lancio, non dopo |
| **6** | **A12–A15** (governo, costi, metodo) | Si scrivono meglio *dopo* aver costruito: saranno pieni di ciò che è successo davvero |
| **7** | Sezione `architect.html` completa + DIARIO-01 + STATO-01 aggiornato | Consolidamento |

**Nota di metodo:** gli step 3–5 sono *lavoro*, non lettura. Procederemo a sessioni: io scrivo il modulo, tu esegui la tappa, mi riporti cosa è successo, il modulo successivo tiene conto di ciò che hai incontrato davvero. È l'unico modo per cui questo corso non diventi teoria.

---

## 7. Tre decisioni che ti chiedo

1. **Il progetto**: Sponsor Tracker (proposto) o un'alternativa del §2?
2. **Il ritmo**: modulo per modulo con la tua esecuzione in mezzo (più lento, molto più efficace) — oppure scrivo l'intero corso subito e tu lo esegui dopo per conto tuo?
3. **L'ambizione**: strumento *interno* (solo il vostro team, semplice) oppure qualcosa che un domani potrebbe diventare un prodotto/servizio (cambia le scelte tecniche fin dall'inizio)?

---

*Changelog: v0.1 — proposta iniziale, da approvare prima dello sviluppo.*
