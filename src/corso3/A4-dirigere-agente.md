# A4 — Dirigere l'agente: context engineering in pratica

**Corso 3 · Parte B · v1.0**

Un agente di codice non è una chat che restituisce testo: legge il repository, modifica file, esegue comandi, legge gli errori, riprova. Cambia tutto — incluso il fatto che può **rompere cose vere**.

Questo modulo è il manuale operativo del rapporto quotidiano con l'esecutore.

---

## 1. Il principio: il contesto è l'unica leva

La qualità del lavoro di un agente dipende quasi interamente da **cosa ha davanti quando decide**. Non dal prompt: dal *contesto complessivo* — file aperti, istruzioni permanenti, storia della sessione, output dei comandi.

Ci sono due modi di sbagliare, simmetrici:

- **Contesto povero** → il modello riempie i buchi con assunzioni plausibili. Inventa una struttura che esiste già altrove, duplica logica, ignora le tue convenzioni.
- **Contesto obeso** → le istruzioni importanti annegano. Un modello con 40 file in contesto e tre pagine di istruzioni segue le regole peggio di uno con 4 file e mezza pagina. Questo è controintuitivo e costa carissimo a chi non lo sa.

> **Il tuo mestiere quotidiano è curare il contesto: dare ciò che serve per questa decisione, e nient'altro.**

---

## 2. I quattro artefatti permanenti

Vivono nel repository, non nella tua testa, e vanno mantenuti come si mantiene un utensile.

### 2.1 Il file di istruzioni del progetto (`CLAUDE.md` o equivalente)

È la costituzione: viene letto a ogni sessione. **Corto e imperativo batte lungo ed esaustivo** — perché ciò che è lungo viene diluito.

```markdown
# Istruzioni di progetto

## Cos'è questo sistema
[3 righe. Serve al modello per fare inferenze giuste sui casi non coperti.]

## Comandi
- test: `npm test`
- avvio locale: `npm run dev`
- migrazioni: `npm run db:migrate`
- controllo tipi + lint: `npm run check`

## Convenzioni
- Il codice è in TypeScript, niente `any`.
- Ogni accesso al DB passa da `src/db/` — mai query sparse nei componenti.
- Gli errori si gestiscono con il tipo `Result`, mai con eccezioni non catturate.
- I test stanno accanto al file che testano.

## Regole non negoziabili
- NON modificare file in `src/db/migrations/` già applicate: crearne di nuove.
- NON aggiungere dipendenze senza chiedere.
- NON toccare `src/billing/` senza esplicita richiesta.
- Prima di modificare più di 3 file, presenta un piano e attendi conferma.

## Dove sono le decisioni
Le scelte architetturali sono in `docs/decisions.md`. Leggile prima di
proporre cambiamenti strutturali. Se una tua proposta le contraddice, dillo
esplicitamente invece di procedere.
```

**La riga più importante è l'ultima delle regole**: costringe l'agente a fermarsi prima delle azioni ampie, che è il punto in cui i disastri si prevengono.

### 2.2 Il Decision Log
Visto in A1. Qui aggiungo la regola d'uso: **va nel contesto quando la sessione è strutturale** (tocca dati, confini, sicurezza), non per una correzione di stile. Metterlo sempre significa consumarlo per niente.

### 2.3 Il piano di sessione
Non un artefatto permanente, ma un rito. Vedi §3.

### 2.4 Il ciclo di feedback automatico
**Questo è l'artefatto che moltiplica tutti gli altri.** Un agente che può eseguire i test, il type-checker e il linter *da solo* opera in un regime completamente diverso: sbaglia, se ne accorge, corregge — senza di te.

Se il tuo progetto non ha un comando unico che verifica tutto (`npm run check && npm test`), **crealo prima di scrivere qualsiasi funzionalità.** È l'investimento con il ritorno più alto dell'intero corso: trasforma l'agente da "produttore di testo plausibile" a "sistema che converge verso il corretto".

---

## 3. Anatomia di una sessione di lavoro

Il ciclo che uso, in cinque fasi. Saltarne una si paga.

### Fase 1 · Delimitare (2 minuti)
Un compito, un risultato. Se non riesci a dire in una frase *cosa sarà vero quando è finito*, il compito è troppo grande.

### Fase 2 · Pianificare, prima di toccare (5 minuti)

```
Compito: [X].
NON scrivere codice. Prima:
1. Elenca i file che intendi modificare e perché.
2. Descrivi l'approccio in 10 righe.
3. Dimmi quali decisioni stai prendendo che io dovrei approvare.
4. Dimmi cosa potrebbe rompersi altrove.
Poi fermati e aspetta.
```

**Correggere un piano costa due minuti; correggere un'implementazione sbagliata costa un'ora — e spesso non ti accorgi nemmeno che era sbagliata.** Il punto 3 è quello che ti restituisce il controllo sulle decisioni che stavi delegando in silenzio.

### Fase 3 · Eseguire a piccoli passi
Un pezzo, verifica, commit. La granularità del commit **è** la granularità della tua capacità di tornare indietro: commit grossi = ritorno indietro grosso = perdi anche il lavoro buono.

### Fase 4 · Verificare (→ A8)
Non "sembra funzionare". Test verdi, diff letto, comportamento provato *sui casi storti*, non su quello felice.

### Fase 5 · Consolidare e chiudere
Prima di chiudere una sessione lunga:

```
Riassumi cosa è cambiato e perché, in forma di voce per il Decision Log:
contesto, decisione, alternative scartate, conseguenze. Elenca separatamente
i debiti che abbiamo contratto (scorciatoie, TODO, casi non gestiti).
```

Questo output va nel Decision Log e nel messaggio di commit. **È il momento in cui la teoria del sistema (→ A1) viene salvata su disco invece di evaporare.**

---

## 4. La gestione del degrado

Una sessione lunga **degrada**: si accumulano tentativi falliti, direzioni abbandonate, correzioni contraddittorie. I sintomi, in ordine di gravità:

1. Il modello reintroduce codice che avevi fatto togliere.
2. "Dimentica" un vincolo dato all'inizio (la convenzione, il divieto).
3. Le proposte diventano generiche, difensive, prolisse.
4. Corregge un errore introducendone un altro, ciclicamente ("loop di riparazione").

**Al sintomo 2 si consolida e si riparte.** Il loop di riparazione (4) è il segnale definitivo: continuare è la scelta peggiore, perché ogni tentativo aggiunge rumore al contesto e riduce la probabilità del tentativo successivo.

**Il protocollo di uscita dal loop:**

```
Fermati. Non tentare un'altra correzione.
Descrivi: 1) cosa dovrebbe accadere, 2) cosa accade davvero, 3) le tre
ipotesi più plausibili sulla causa, in ordine di probabilità, 4) per
ciascuna, l'esperimento minimo che la conferma o la esclude.
Non implementare nulla: voglio scegliere io l'esperimento.
```

Poi **chiudi la sessione e riapri pulito**, portandoti dietro solo la diagnosi. Nove volte su dieci il problema si risolve in cinque minuti nella sessione nuova — perché il rumore era il problema.

---

## 5. Modalità di lavoro: quando usare cosa

| Modalità | Quando | Rischio |
|---|---|---|
| **Chat (codice a schermo)** | Capire, valutare opzioni, imparare, revisionare | Nessuno: non tocca nulla |
| **Agente con approvazione manuale** | Lavoro normale su codice che ti importa | Basso: vedi ogni azione |
| **Agente autonomo** (esegue senza chiedere) | Compiti meccanici e verificabili: migrazioni, test, refactoring con suite verde | **Alto se il ciclo di feedback è debole**: si allontana in fretta |
| **Più agenti in parallelo** | Compiti indipendenti su aree separate | Alto: conflitti, e tu perdi la visione d'insieme |

**Regola:** l'autonomia è proporzionale alla forza del ciclo di feedback, non alla tua fretta. Un agente autonomo su un progetto senza test è un generatore di debito ad alta velocità.

---

## 6. Il prompt che uso più di tutti

Non è un prompt di produzione. È di comprensione:

```
Spiegami cosa fa questo codice come se dovessi difenderlo in una revisione
avversariale. In particolare: quali assunzioni fa sul resto del sistema,
quali input lo romperebbero, e cosa succede se viene eseguito due volte in
parallelo.
```

Serve a mantenere viva la **teoria del sistema**. Il codice che non capisci non è tuo: è un debito con interessi, e prima o poi lo paghi in un momento che sceglierà lui.

---

## Strumenti di questo modulo

1. **File di istruzioni di progetto** — modello completo, con la regola "piano prima di toccare 3+ file".
2. **Comando unico di verifica** — l'investimento con il ritorno più alto.
3. **Il ciclo in 5 fasi** — delimitare, pianificare, eseguire, verificare, consolidare.
4. **Il prompt del piano** — con "quali decisioni sto delegando senza saperlo".
5. **Il protocollo di uscita dal loop di riparazione.**
6. **Il prompt di comprensione** — per non perdere la teoria del sistema.

---

## Errori tipici

1. **Far partire l'agente senza piano.** È il 90% dei disastri, ed è sempre per fretta.
2. **Sessioni-fiume.** Producono sistemi che nessuno capisce, incluso chi li ha commissionati.
3. **Autonomia senza test.** Velocità verso il posto sbagliato.
4. **Istruzioni chilometriche.** Diluiscono ciò che conta. Corto e imperativo.
5. **Insistere nel loop di riparazione.** Ogni tentativo peggiora il successivo.
6. **Non consolidare.** La sessione finisce, la conoscenza evapora, e fra un mese leggi il tuo stesso codice come se l'avesse scritto un estraneo. Perché, in un certo senso, è così.

*v1.0 — riscrittura in profondità.*
