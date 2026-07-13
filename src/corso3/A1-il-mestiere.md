# A1 — Il mestiere: cosa fai tu, cosa fa la macchina

**Corso 3 · Parte A · v1.0**

---

## 1. La ridistribuzione del lavoro

Un progetto software è fatto di sei attività distinte. Prima le facevano persone diverse; il punto non è che ora "l'AI programma", è che **la distribuzione è cambiata in modo asimmetrico**.

| Attività | Chi la faceva | Chi la fa ora | Costo marginale oggi |
|---|---|---|---|
| Decidere cosa costruire | Committente | **Tu** | Alto (è pensiero) |
| Progettare la struttura | Architetto/senior | **Tu + AI** | Medio |
| Scrivere il codice | Sviluppatori | **AI** | ~Zero |
| Verificare che sia giusto | Sviluppatori + QA | **Tu** (con strumenti) | **Alto e crescente** |
| Mantenere nel tempo | Team | **Tu** | Alto |
| Capire il sistema | Il team, collettivamente | **Solo tu** | **Altissimo** |

Le due righe in grassetto sono l'intero corso. Il codice è diventato gratis; **la comprensione no**. E la comprensione è ciò che ti permette di cambiare il sistema domani.

### La legge che governa tutto il resto

> **Puoi generare codice a una velocità superiore a quella con cui puoi comprenderlo. Ogni riga che non comprendi è debito che maturerà interessi.**

Non è una massima morale. È una constatazione operativa: il collo di bottiglia si è spostato dalla produzione alla **verifica e alla comprensione**, e tutti gli strumenti di questo corso servono a spostarlo di nuovo — automatizzando la verifica, riducendo ciò che *devi* capire, e rendendo esplicito ciò che altrimenti resterebbe nella tua testa.

---

## 2. Le tre asimmetrie di un esecutore-AI

Dirigere un modello non è dirigere uno sviluppatore junior veloce. Le differenze non sono di grado, sono di natura.

### 2.1 Asimmetria di memoria

Uno sviluppatore umano, dopo tre mesi sul progetto, *sa* perché avete scelto Postgres invece di Mongo, perché quella funzione ha quel workaround brutto, perché non si tocca il modulo dei pagamenti il venerdì. Un modello **non sa nulla di tutto ciò** se non glielo metti davanti, ogni volta.

**Conseguenza pratica:** le decisioni architetturali che vivono solo nella tua testa **verranno silenziosamente violate**. Non per cattiveria: per assenza. Il modello riscriverà quel workaround "pulendolo", e romperà la cosa che il workaround proteggeva.

**Strumento — il Decision Log (ADR leggero).** Un file, `docs/decisions.md`, una voce per decisione:

```markdown
## 0007 · Niente soft-delete sugli iscritti — 2026-03-14
**Contesto:** serviva cancellare iscritti errati senza perdere lo storico ECM.
**Decisione:** cancellazione reale + tabella `iscritti_archiviati` con motivo e data.
**Alternative scartate:** flag `deleted_at` — scartato perché ogni query avrebbe
dovuto ricordarsi di filtrarlo, e prima o poi una se ne dimentica.
**Conseguenze:** chi legge lo storico deve fare UNION su due tabelle. Accettato.
**Stato:** attiva
```

Questo file va **nel contesto dell'agente a ogni sessione strutturale** (non per le modifiche banali). È il singolo artefatto che più riduce il danno dell'asimmetria di memoria. Costo: due minuti a decisione.

### 2.2 Asimmetria di velocità

Il modello produce 800 righe mentre tu ne leggi 40. Se accetti il ritmo che ti impone, entro un mese avrai un sistema che non conosci. **Il ritmo lo detti tu**, e si detta in un solo modo: **limitando la dimensione dell'unità di lavoro.**

**Strumento — la regola del diff leggibile.** Nessuna singola richiesta all'agente deve produrre un diff che tu non riesca a leggere in 10 minuti. In pratica: 200–400 righe. Se il compito è più grande, non è un compito: è un progetto, e va spezzato prima di essere assegnato.

Quando il diff supera la soglia, hai due opzioni: spezzare, oppure **accettare consapevolmente di non capire quel pezzo** — cosa legittima per codice generato ripetitivo (migrazioni, boilerplate, test), inaccettabile per logica di dominio. La distinzione è tua.

### 2.3 Asimmetria di dissenso

Un senior ti dice "questa è una cattiva idea". Il modello, se gli chiedi di implementare una cattiva idea, la implementa — bene, in fretta, con test. Non c'è nessun attrito naturale contro le decisioni sbagliate.

**Strumento — il dissenso lo devi costruire tu.** Tre meccanismi, in ordine di forza:

1. **Il ruolo avversariale esplicito**, in una sessione separata (non nella stessa conversazione: chi ha prodotto difende, è un fatto osservabile e non un difetto morale).
2. **Il pre-mortem sulla decisione** (§ A3).
3. **La domanda strutturale**, che uso a ogni decisione non banale:

```
Prima di implementare: elencami le tre ragioni più forti per NON fare
quello che ho chiesto. Se una di queste ragioni è decisiva, dimmelo
chiaramente invece di procedere. Se nessuna lo è, spiegami perché e
procedi.
```

---

## 3. Cosa resta irriducibilmente tuo

Non per orgoglio umanistico: per struttura del problema.

**La definizione del problema.** Nessun modello sa quale problema valga la pena risolvere, perché la risposta dipende da informazioni che non sono nel testo: i tuoi clienti, la tua economia, il tuo tempo, cosa succede se sbagli.

**I compromessi con conseguenze fuori dal sistema.** "Facciamo prima e sistemiamo dopo" è una decisione che coinvolge il tuo tempo futuro, la reputazione con un cliente, il rischio legale. Il modello può elencarti i trade-off tecnici; non può pesarli, perché i pesi sono tuoi.

**L'accettazione.** Dire "questo è finito" è un atto di responsabilità, non una misura tecnica. Nessuna suite di test ti autorizza a firmare.

**La teoria del sistema.** Peter Naur, nel 1985 (*Programming as Theory Building*), sostenne una cosa che oggi diventa decisiva: il programma non è il codice — il programma è **la teoria che i programmatori hanno in testa** su come il mondo si mappa su quel codice. Il codice è il residuo. Quando la teoria muore (il team se ne va), il codice diventa immanutenibile *anche se è perfettamente leggibile*, perché nessuno sa più *perché* è così, e quindi nessuno sa quali modifiche sono sicure e quali no.

**Con un esecutore-AI, la teoria non si costruisce da sola.** Il modello non la possiede tra una sessione e l'altra. Se non vive in te — e nei documenti che scrivi — non vive da nessuna parte. Questa è la ragione profonda per cui il Decision Log non è burocrazia: **è il supporto materiale della teoria del sistema.**

---

## 4. Strumento: la Carta di progetto

Un solo documento, una pagina, scritto prima di ogni progetto. Non è un piano: è un **vincolo contro te stesso fra tre mesi**, quando sarai stanco e tentato di dire sì a tutto.

```markdown
# Carta · [NOME]

## Problema
Chi soffre, oggi, di cosa. Nominare una persona reale, non una categoria.

## Cosa è
Tre righe. Se ne servono dieci, non hai capito cosa stai costruendo.

## Cosa NON è
(Questa sezione deve essere lunga almeno quanto la precedente.)

## Utenti e beneficio
Per ciascuno: cosa fa oggi senza · cosa farà con · perché cambierebbe abitudine.

## Criteri di morte
Cosa deve accadere perché io decida di ucciderlo. Scriverlo ORA, quando sono lucido.

## Come saprò che funziona
Segnali osservabili da un estraneo. Non "i clienti sono contenti".

## Cosa succede se sparisce
Determina il livello di rigore che devo: se qualcuno perde dati, backup e
sicurezza sono il prodotto, non un dettaglio.
```

**I "criteri di morte" sono la parte che nessuno scrive e che salva più tempo di ogni altra.** Un progetto senza condizioni di uscita non finisce mai: si trascina, consuma, e occupa il posto della cosa giusta.

---

## 5. Il modello mentale del rapporto

Chiudo con l'analogia che regge meglio, perché ti è familiare: **dirigere un esecutore-AI assomiglia a dirigere un congresso**, non a dirigere un dipendente.

Non tieni tu le relazioni scientifiche. Ma: decidi il programma (specifica), scegli chi parla di cosa (architettura), imponi i tempi (vincoli), verifichi che ciò che arriva sia all'altezza (revisione), e **ti prendi la responsabilità del risultato davanti a 300 persone** (accettazione). Non serve essere anestesista per dirigere Romanestesia — serve saper riconoscere una sessione mal costruita.

Con il software è identico. **Non devi saper scrivere il codice. Devi saper riconoscere un sistema mal costruito** — e il resto del corso è l'addestramento di quel riconoscimento.

---

## Strumenti di questo modulo

1. **Decision Log** (`docs/decisions.md`) — contro l'asimmetria di memoria.
2. **Regola del diff leggibile** (200–400 righe) — contro l'asimmetria di velocità.
3. **Prompt del dissenso strutturale** — contro l'asimmetria di compiacenza.
4. **Carta di progetto**, con criteri di morte — contro te stesso.

---

## Errori tipici

1. **Confondere la produttività con il progresso.** 5.000 righe generate in un'ora non sono progresso se non sai dire cosa fanno.
2. **Tenere le decisioni in testa.** Funziona finché il progetto è piccolo. Poi smette, e non te ne accorgi subito.
3. **Chiedere conferma invece di dissenso.** "Ti sembra una buona idea?" ha una sola risposta, e non è informativa.
4. **Rimandare i criteri di morte.** Chi non decide come si ferma, non si ferma.

*v1.0 — riscrittura in profondità.*
