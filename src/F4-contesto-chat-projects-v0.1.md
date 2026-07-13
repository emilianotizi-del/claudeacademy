# F4 — Contesto, chat e Projects

**Codice modulo:** F4 · **Versione:** 0.1 · **Data:** 13/07/2026
**Dipendenze:** F1, F2, F3, DATI-01 v1.0
**Richiamato da:** F5, F6 · Corso 1 blocchi 2.2 e 2.5 · Manuale M4–M6
**Etichette:** [FATTO] documentato · [BP] best practice consolidata · [OPINIONE] dibattuta · [SUGG] suggerimento del team
**Fonti verificate il 13/07/2026:** support.claude.com — articoli "What are projects?", "How can I create and manage projects?", "Use Claude's chat search and memory". Le funzionalità evolvono: in caso di dubbio fa fede la documentazione ufficiale.

---

## 1. Scopo del modulo

Al termine di questo modulo saprai:

- capire **cos'è il contesto** e perché decide la qualità delle risposte;
- scegliere **quando aprire una nuova chat** e quando serve un **Project**;
- **costruire e organizzare i Projects** di B Best/HERA, incluse condivisione nel team e memoria.

---

## 2. Il principio

> **Ciò che non è nel contesto, per Claude non esiste. Il Project è il posto dove il contesto vive stabilmente.**

Da → F1 §4.2 sai che ogni chat parte (quasi) da zero. Questo modulo è la soluzione organizzata a quel limite.

---

## 3. Cos'è il contesto

Il **contesto** è tutto ciò che Claude "ha davanti" mentre risponde. In una chat di claude.ai è composto da:

1. **la conversazione corrente** — tutto ciò che vi siete detti in quella chat;
2. **i documenti allegati** alla chat (→ F5);
3. **le istruzioni e la conoscenza del Project**, se la chat è dentro un Project (§5);
4. **la memoria**, se attiva (§7).

Due proprietà pratiche:

- **Il contesto è capiente ma non infinito.** [FATTO] Conversazioni e documenti molto lunghi possono avvicinarsi al limite; nei Projects a pagamento, quando la conoscenza si avvicina al limite, il sistema passa automaticamente a una modalità di recupero (RAG) che estrae i passaggi rilevanti invece di tenere tutto "davanti agli occhi".
- **Un contesto lungo e disordinato peggiora le risposte.** [BP] Correzioni contraddittorie, tentativi falliti e argomenti mescolati "inquinano" la conversazione: Claude trascina detriti dei passaggi precedenti. È il motivo della regola delle tre correzioni di → F3.

---

## 4. Quando aprire una nuova chat

**[BP]** Regola base: **una chat = un compito** (o un tema strettamente connesso). Segnali che è ora di aprirne una nuova:

1. **Cambio di argomento** — dalla mail alla faculty al preventivo del catering: due chat.
2. **Chat lunga e "stanca"** — risposte che peggiorano, dettagli dimenticati, vecchie versioni che riaffiorano.
3. **Incanalamento sbagliato** — dopo vari fraintendimenti, meglio ribriefare da zero portandosi dietro solo ciò che si è imparato (→ F3 §3.1).

**[FATTO]** Attenzione a un equivoco frequente: **le chat dentro uno stesso Project non condividono il contesto tra loro.** Ogni chat vede istruzioni e conoscenza del Project, ma non le altre conversazioni. Se una chat produce una conclusione che deve servire alle successive, va salvata nella conoscenza del Project (o gestita tramite memoria, §7).

---

## 5. I Projects

**[FATTO]** Un Project è uno spazio di lavoro che raggruppa: **istruzioni personalizzate** (applicate a ogni chat del Project), una **conoscenza di progetto** (documenti e testi consultabili da ogni chat) e le **chat** relative. Sui piani Team/Enterprise i Projects si possono **condividere** con i colleghi, con due livelli: *può usare* (vede e chatta) e *può modificare* (interviene su istruzioni e conoscenza). L'archiviazione di un Project azzera le condivisioni.

### 5.1 Quando creare un Project

**[SUGG]** Merita un Project ciò che ha vita lunga e materiale proprio:

- **un evento** (Romanestesia 2026): programma, faculty, sponsor, logistica;
- **un processo ricorrente** (amministrazione eventi, comunicazione);
- **un patrimonio comune** (la libreria prompt di → F2 §7).

Non merita un Project il compito singolo: per sistemare una mail basta una chat.

### 5.2 Le istruzioni giuste

**[BP]** Le istruzioni di Project dicono a Claude, una volta per tutte: chi siete, che ruolo deve avere, il tono, le regole fisse. Corte e nette battono lunghe ed esaustive.

**Esempio — istruzioni per il Project "Romanestesia 2026":**

```
Sei l'assistente della segreteria organizzativa di B Best per Romanestesia
2026 (XXII edizione, 11–12 dicembre, Auditorium Antonianum, Roma; ~300
partecipanti; provider ECM: Hera Foundation; piattaforma: Hera Education).
Tono professionale e cordiale; italiano, salvo richiesta diversa.
Regole fisse: ogni nome, data, numero e norma va segnalato come "da
verificare" se non proviene dai documenti del progetto; le bozze di quiz
ECM riportano sempre la dicitura "da validare dal responsabile scientifico";
non inserire mai dati personali non necessari negli output.
```

### 5.3 La conoscenza giusta

**[SUGG]** Nella conoscenza del Project evento mettete: la scheda dati (DATI-01), il programma aggiornato, i template della libreria pertinenti, le procedure rilevanti. **Pochi documenti buoni e aggiornati battono quaranta file alla rinfusa** [BP]: la conoscenza è una dispensa, non una cantina.

Manutenzione: quando un documento cambia (programma v2), **sostituite** il vecchio — due versioni dello stesso file nella conoscenza sono una fabbrica di risposte contraddittorie.

---

## 6. L'organizzazione proposta per B Best/HERA

**[SUGG]** Mappa di partenza, da adattare con l'uso:

| Project | Contenuto | Owner |
|---|---|---|
| **Romanestesia 2026** (uno per evento) | Scheda dati, programma, faculty, sponsor, logistica | Referente evento |
| **Segreteria & Libreria prompt** | Template di → F2, tono aziendale, firme | Direzione |
| **Amministrazione eventi** | Procedure Fattureincloud, template preventivi/solleciti | Amministrazione |
| **Comunicazione** | Linee editoriali, esempi di post/newsletter riusciti | Comunicazione |

Regole di convivenza:

1. **Un owner per Project** — decide cosa entra nella conoscenza.
2. **Nomi parlanti** — "Romanestesia 2026", non "Congresso" (tra un anno ce ne saranno tre).
3. **La libreria prompt vive in un solo posto** — gli altri Project la richiamano, non la duplicano.
4. **A evento concluso, si archivia** — ordine, e le condivisioni si azzerano.

---

## 7. La memoria

**[FATTO]** Oltre ai Projects, claude.ai offre (sui piani a pagamento) la **ricerca nelle chat passate** e la **memoria**: una sintesi automatica delle conversazioni, aggiornata circa ogni 24 ore, che dà continuità alle nuove chat. **Ogni Project ha uno spazio di memoria separato**, così il contesto di un evento non si mescola con il resto. Si controlla da Impostazioni → Capacità: si può mettere in pausa o azzerare; le chat in incognito ne sono escluse. Sui piani Enterprise l'amministratore può gestirla a livello di organizzazione.

**[SUGG]** Regola d'uso: la memoria è un comfort, non un archivio. **Ciò che è critico va nella conoscenza del Project**, dove è esplicito, visibile e modificabile; alla memoria lasciate la continuità spicciola.

---

## 8. Errori tipici

1. **Il Project-cestino**: tutto dentro un unico Project, dalla faculty alle ferie.
2. **Istruzioni chilometriche**: due pagine di regole che si contraddicono; le istruzioni sono un cartello, non un manuale.
3. **Conoscenza fossile**: il programma di marzo ancora lì a dicembre.
4. **Aspettarsi che le chat del Project si "parlino"**: non lo fanno (§4) — le conclusioni utili si depositano nella conoscenza.
5. **Dati sensibili nella conoscenza condivisa**: anagrafiche complete, contratti riservati — prima i criteri di → F6.
6. **Affidare alla memoria ciò che deve essere certo**: la memoria sintetizza a modo suo; il vincolo esplicito va nelle istruzioni.

---

## 9. Esercizio (blocco 2.2)

In aula costruiremo insieme il Project "Romanestesia 2026": scrittura delle istruzioni (partendo dal template del §5.2), caricamento della scheda dati e del programma, prima chat di prova per ruolo, condivisione con il team con i permessi giusti.

---

## 10. Punti chiave del modulo

1. Il contesto è tutto ciò che Claude ha davanti: conversazione, allegati, Project, memoria.
2. Una chat = un compito; chat lunga e stanca = chat nuova.
3. Le chat di un Project **non** condividono il contesto tra loro: ciò che serve a tutte va nella conoscenza.
4. Project = istruzioni + conoscenza + chat; si condivide nel team con permessi.
5. Istruzioni corte e nette; conoscenza poca e aggiornata.
6. Un Project per evento, più i trasversali; un owner ciascuno.
7. La memoria dà continuità, la conoscenza dà certezza: il critico sta nella conoscenza.
8. Prima di caricare dati nella conoscenza condivisa: → F6.

---

## 11. Collegamenti

- **← F1** (le chat partono da zero) · **← F3** (contesto inquinato) · **→ F5** (documenti) · **→ F6** (dati e condivisione)
- **Corso 1**: blocchi 2.2 e 2.5 · **Manuale**: M4, M5, M6

---

*Changelog: v0.1 — prima stesura; fatti di prodotto verificati su support.claude.com il 13/07/2026; organizzazione Projects proposta su DATI-01 v1.0.*
