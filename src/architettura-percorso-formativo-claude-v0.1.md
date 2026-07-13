# Architettura del Percorso Formativo Claude — B Best & HERA

**Codice documento:** ARCH-00 · **Versione:** 0.1 (bozza per approvazione) · **Data:** 13/07/2026
**Stato:** in attesa di approvazione — nessun contenuto verrà sviluppato prima del via libera

---

## 0. Come leggere questo documento

Il documento presenta, nell'ordine richiesto:

1. Struttura complessiva del sistema formativo (§1–2)
2. Indici dettagliati dei tre prodotti (§3–5)
3. Ordine degli argomenti e sequenza didattica (§6)
4. Motivazione delle scelte (§7)
5. Modifiche e integrazioni che consigliamo (§8)

In tutto il progetto useremo quattro etichette per distinguere la natura delle affermazioni:

| Etichetta | Significato |
|---|---|
| **[FATTO]** | Documentato ufficialmente da Anthropic (con link alla fonte) |
| **[BP]** | Best practice consolidata nella community e nella letteratura sul prompting |
| **[OPINIONE]** | Opinione diffusa ma dibattuta; esistono approcci alternativi validi |
| **[SUGG]** | Suggerimento del team formativo, calibrato sul contesto B Best/HERA |

---

## 1. Principio architetturale: un sistema, non tre corsi

**[SUGG]** Il rischio maggiore di un progetto con tre prodotti è la duplicazione: gli stessi concetti (prompting, contesto, verifica) servono sia al personale amministrativo sia all'AI Software Architect. Per evitarlo, proponiamo un'architettura a tre livelli:

```
LIVELLO 1 — FONDAMENTA COMUNI (moduli F)
   Concetti trasversali scritti UNA volta sola
   ↓ richiamati da ↓
LIVELLO 2 — MANUALE DI RIFERIMENTO (moduli M)     ← Prodotto 2
   Ricette operative + approfondimenti + reference
   ↓ da cui derivano ↓
LIVELLO 3 — PERCORSI DIDATTICI
   Corso 1 (aula, 8h)         ← selezione + esercizi dai moduli F/M
   Corso 3 (AI Sw Architect)  ← moduli A, che estendono F senza riscriverli
```

Regole di sistema:

- **Ogni concetto vive in un solo modulo.** Gli altri materiali lo richiamano con riferimento esplicito (es. "→ vedi F2 §3").
- **Ogni modulo è autonomo e versionato**: intestazione con codice, versione, data, dipendenze, changelog. Quando Anthropic introduce una novità, si aggiorna un modulo, non l'intero corpus.
- **Le slide del Corso 1 non contengono teoria propria**: sono la proiezione didattica dei moduli F/M. Se cambia il modulo, si sa esattamente quale slide aggiornare.

---

## 2. I moduli Fondamenta (F) — nucleo condiviso

| Codice | Titolo | Contenuto essenziale |
|---|---|---|
| **F1** | Come ragiona Claude | Cosa è (e non è) un modello linguistico; punti di forza e limiti; perché "capisce" il contesto ma non "sa" cosa è successo ieri; allucinazioni: cosa sono e perché nascono |
| **F2** | Scrivere buone richieste | Anatomia di un buon prompt: contesto, obiettivo, formato, vincoli, esempi; il principio "briefing a un collaboratore nuovo" |
| **F3** | Iterare, correggere, verificare | La conversazione come processo; come chiedere revisioni; come correggere senza ripartire da zero; verifica delle informazioni e gestione delle allucinazioni |
| **F4** | Contesto, chat e Projects | Cos'è il contesto; quando aprire una nuova chat; quando serve un Project; memoria e conoscenza di Progetto; organizzazione dei Projects |
| **F5** | Lavorare con i documenti | Caricare PDF/Word/Excel; analisi, sintesi, estrazione; limiti pratici; buone abitudini di gestione documentale |
| **F6** | Dati, riservatezza e responsabilità | Cosa caricare e cosa no (dati personali, dati sanitari, dati sponsor, GDPR/ECM); l'output è sempre responsabilità di chi lo firma |

**[SUGG]** F6 non era esplicito nel brief, ma per un'azienda che gestisce congressi medici, ECM, faculty e sponsor è indispensabile e va inserito già nel Corso 1 (vedi §8).

---

## 3. Prodotto 1 — Corso "Claude per B Best e HERA" (aula, 2 pomeriggi)

### 3.1 Impianto didattico

**[BP]** Per adulti con competenze eterogenee funziona il ciclo *caso reale → prova guidata → concetto → prova autonoma*. Ogni blocco parte da un'attività vera dell'azienda; la funzionalità di Claude emerge come soluzione, mai come argomento a sé.

### 3.2 Giornata 1 — "Claude come collaboratore quotidiano" (~4h)

| Blocco | Durata | Caso pratico trainante | Moduli richiamati |
|---|---|---|---|
| 1.1 Che collaboratore è Claude | 40' | Demo dal vivo: sistemare una mail in inglese a un relatore straniero — prima male, poi bene | F1 |
| 1.2 Il briefing perfetto | 60' | Riscrivere una comunicazione a sponsor in tre versioni (formale, sintetica, persuasiva) | F2 |
| 1.3 Iterare e correggere | 50' | Sintesi di un verbale di riunione: dalla prima bozza alla versione finale in 3 passaggi | F3 |
| *Pausa* | 15' | | |
| 1.4 Documenti alla mano | 60' | Analizzare il PDF di un programma scientifico: estrarre faculty, orari, incongruenze | F5 |
| 1.5 Fidarsi ma verificare | 30' | Caccia all'allucinazione: trovare gli errori in un output volutamente imperfetto | F3, F1 |
| 1.6 Chiusura + compito ponte | 15' | Ognuno porta per la G2 un compito reale del proprio ruolo | — |

### 3.3 Giornata 2 — "Dal singolo compito al flusso di lavoro" (~4h)

| Blocco | Durata | Caso pratico trainante | Moduli richiamati |
|---|---|---|---|
| 2.1 Ripresa + debrief compiti | 30' | Discussione dei compiti ponte | — |
| 2.2 Chat, contesto e Projects | 60' | Costruire insieme il Project "Congresso [nome reale]": istruzioni, documenti, prime chat | F4 |
| 2.3 Laboratorio per ruoli | 75' | Tracce differenziate: segreteria scientifica (programma + ECM), eventi (logistica + fornitori), comunicazione (piano editoriale + comunicati), amministrazione (report + procedure) | F2–F5 |
| *Pausa* | 15' | | |
| 2.4 Dati e responsabilità | 30' | Casi limite reali: "posso caricare questo?" (anagrafiche faculty, contratti sponsor, dati ECM) | F6 |
| 2.5 Lavorare insieme a Claude | 30' | Condividere Projects e prompt tra colleghi; la libreria prompt aziendale | F4 |
| 2.6 Esercitazione finale + patto d'uso | 30' | Mini-progetto per ruolo + quiz + consegna del compito finale | — |

### 3.4 Materiali da produrre per il Corso 1

1. Indice generale e programma delle due giornate (questa sezione, raffinata)
2. Slide in formato testo, blocco per blocco (poi convertibili in .pptx)
3. Note del docente per ogni blocco (timing, domande frequenti in aula, piani B)
4. Esercizi con tracce differenziate per ruolo
5. Casi pratici completi con soluzioni commentate
6. Quiz di fine giornata (G1 e G2)
7. Compito finale: applicare Claude a un processo reale del proprio ruolo, con griglia di valutazione

---

## 4. Prodotto 2 — Manuale di riferimento

### 4.1 Struttura in sette parti

**PARTE I — Capire Claude** *(= moduli F1–F3 in forma estesa)*
- M1. Come funziona Claude e cosa aspettarsi (F1 esteso)
- M2. L'arte della richiesta (F2 esteso: pattern, template, prompt annotati buoni/cattivi)
- M3. Conversare, iterare, verificare (F3 esteso: tecniche di revisione, anti-allucinazione)

**PARTE II — Organizzare il lavoro**
- M4. Chat, contesto e memoria (F4 esteso)
- M5. Projects: progettazione, istruzioni, documenti, manutenzione
- M6. Collaborare: condivisione, libreria prompt aziendale, standard interni

**PARTE III — Ricette operative B Best/HERA** *(cuore del manuale: schede autonome, tutte con struttura identica: quando serve → prompt di partenza → iterazioni tipiche → errori comuni → variante avanzata)*
- M7. Email e corrispondenza (relatori, sponsor, faculty, provider ECM)
- M8. Documenti congressuali (programmi scientifici, abstract book, regolamenti)
- M9. Sintesi e analisi (paper, verbali, report, capitolati)
- M10. Traduzioni e revisione linguistica (IT↔EN, registro scientifico)
- M11. Presentazioni e materiali di comunicazione
- M12. Organizzazione e project management (timeline congresso, checklist, follow-up riunioni)
- M13. Procedure interne e knowledge management (trasformare il "come si fa" in documentazione)
- M14. Integrazione con Google Workspace (Gmail, Calendar, Drive) — cosa è possibile oggi, con verifica sulla documentazione ufficiale

**PARTE IV — Qualità e sicurezza**
- M15. Verificare, controllare, firmare (responsabilità sull'output)
- M16. Dati, privacy, riservatezza (F6 esteso: casistica ECM/sanitaria/sponsor)

**PARTE V — Riferimenti rapidi**
- M17. Cheat sheet (1 pagina per ruolo)
- M18. Checklist operative (avvio congresso, chiusura evento, revisione documenti…)
- M19. Errori frequenti e come evitarli
- M20. FAQ
- M21. Glossario

**PARTE VI — Aggiornamenti**
- M22. Registro delle novità Anthropic e impatto sui moduli (changelog vivo)

**PARTE VII — Appendici**
- Libreria prompt aziendale (documento vivo, alimentato anche dai corsisti)

### 4.2 Convenzioni

- Ogni scheda della Parte III è **indipendente**: leggibile senza aver letto il resto, con rimandi espliciti ai moduli F per la teoria.
- Ogni affermazione su funzionalità di prodotto sarà **[FATTO]** con link a documentazione ufficiale Anthropic (docs.claude.com / support.claude.com), verificata al momento della stesura — non basata su memoria.

---

## 5. Prodotto 3 — Corso "AI Software Architect"

### 5.1 Filosofia

**[SUGG]** Il percorso è costruito attorno a un cambio di identità professionale: da "persona che chiede codice" a **direttore tecnico di un team di AI**. Ogni modulo risponde alla domanda: *cosa farebbe un buon architetto/CTO in questa situazione?* Il codice compare solo come oggetto da dirigere, revisionare e governare — mai da scrivere riga per riga.

### 5.2 Percorso in quattro fasi (moduli A)

**FASE 1 — Il mestiere del direttore (intermedio)**
- A1. Mentalità: dirigere vs. delegare vs. abdicare; dove l'AI eccelle e dove fallisce nello sviluppo software
- A2. La documentazione come strumento di comando: Architecture Bible, Product Bible, Roadmap, Task management — cosa contengono, come si scrivono, come Claude le usa
- A3. Dividere un progetto: decomposizione modulare, confini, interfacce, criteri di "task ben posto" per un'AI

**FASE 2 — Governare il contesto (intermedio-avanzato)**
- A4. Context engineering: cosa mettere nel contesto, quando, in che ordine; economia dei token
- A5. Context pollution: riconoscerla, prevenirla, ripulirla; quando chiudere una sessione e ripartire
- A6. Repository strategy: struttura del repo pensata per l'AI; convenzioni; gestione di repository grandi

**FASE 3 — Gli strumenti al posto giusto (avanzato)**
- A7. Chat vs. Claude Code vs. Projects: matrice decisionale (quale strumento per quale fase del lavoro), con confronto esplicito di vantaggi, limiti e contesti d'uso
- A8. Claude Code in pratica: setup, CLAUDE.md, workflow quotidiano, VS Code
- A9. Integrazioni: GitHub, Vercel, Supabase — flussi reali di sviluppo e deploy
- A10. Collaborazione tra più AI: quando ha senso, pattern (pianificatore/esecutore, revisore incrociato), rischi **[OPINIONE: area dibattuta, presenteremo approcci a confronto]**

**FASE 4 — Qualità e regime (avanzato)**
- A11. Debugging diretto dall'architetto: strategie di diagnosi, come far isolare il problema a Claude
- A12. Code review assistita: cosa controllare sempre, checklist, quando non fidarsi
- A13. Riduzione errori e consumo token: pattern economici, batch di lavoro, sessioni corte vs. lunghe (confronto)
- A14. Anti-pattern: catalogo ragionato degli errori tipici di chi dirige male un'AI
- A15. Casi studio completi: un progetto reale dall'idea al deploy, condotto con il metodo del corso

Ogni fase include: workflow descritti passo-passo, diagrammi testuali, checklist, esercizi pratici su un progetto-palestra ricorrente.

---

## 6. Ordine di sviluppo dei contenuti

**[SUGG]** Proponiamo questa sequenza di produzione:

1. **Moduli F1–F6** — sono il nucleo da cui tutto dipende
2. **Corso 1 completo** (slide, note docente, esercizi, quiz) — è quello con scadenza d'aula
3. **Manuale, Parti I–II** (rapide: estensione dei moduli F)
4. **Manuale, Parte III** (le ricette: la parte più lunga, sviluppabile a lotti per ruolo)
5. **Manuale, Parti IV–VII**
6. **Corso 3**, fase per fase

Vantaggio: dopo i passi 1–2 hai già tutto il necessario per erogare l'aula; il resto cresce senza bloccarti.

---

## 7. Motivazione delle scelte principali

1. **Fondamenta condivise (F)** — Elimina le duplicazioni alla radice e rende gli aggiornamenti chirurgici: una novità di Anthropic tocca un modulo F o una scheda M, mai tre documenti in parallelo.
2. **Caso pratico prima del concetto** — **[BP]** consolidata nell'andragogia (formazione degli adulti): l'adulto apprende quando riconosce il proprio problema. Per questo ogni blocco d'aula apre con un'attività vera di B Best/HERA, non con una funzionalità.
3. **Laboratorio per ruoli in G2** — Con destinatari così eterogenei (segreteria scientifica vs. amministrazione), un esercizio unico annoierebbe metà aula. Tracce differenziate mantengono tutti su casi propri.
4. **Projects in Giornata 2, non in Giornata 1** — **[SUGG]** I Projects hanno senso solo dopo aver interiorizzato cos'è il contesto. Introdurli prima produce cargo-cult ("creo un Project perché me l'hanno detto") invece di comprensione.
5. **Schede operative a struttura fissa (Parte III)** — La ripetitività della struttura (quando serve → prompt → iterazioni → errori → variante) è una scelta deliberata: dopo la terza scheda, chi legge sa già dove trovare cosa.
6. **Corso 3 organizzato per fasi di maturità e non per strumenti** — Un indice "capitolo GitHub, capitolo Supabase" descriverebbe funzionalità; l'obiettivo dichiarato è invece imparare a *dirigere*. Gli strumenti compaiono nella Fase 3, quando il metodo è già impostato.
7. **Verifica sistematica sulle fonti ufficiali** — Le funzionalità di Claude evolvono rapidamente; ogni affermazione di prodotto sarà verificata su docs.claude.com / support.claude.com al momento della stesura e marcata **[FATTO]** con link, per rendere il materiale auditabile e aggiornabile.

---

## 8. Modifiche e integrazioni che consigliamo

Tutte **[SUGG]**, da approvare o scartare:

1. **Aggiungere il modulo F6 (dati e riservatezza)** e il blocco 2.4 in aula. Contesto ECM/sanitario/sponsor: il rischio reputazionale di un uso ingenuo è concreto. *Costo: ~30' d'aula.*
2. **Questionario pre-corso** (5 domande, 10 minuti): ruolo, uso attuale dell'AI, un compito che vorrebbero delegare. Permette di calibrare esempi e formare i gruppi del laboratorio 2.3. 
3. **Libreria prompt aziendale come deliverable vivo**: nasce in aula (blocco 2.5), vive nel manuale (Parte VII), cresce nel tempo. È il ponte più efficace tra corso e uso quotidiano.
4. **"Compito ponte" tra le due giornate** (blocco 1.6): ognuno arriva in G2 con un caso proprio. Trasforma la G2 da lezione a consulenza collettiva.
5. **Registro aggiornamenti (M22)** con revisione trimestrale: dato l'obiettivo di mantenere il percorso nel tempo, serve un meccanismo esplicito, non la buona volontà.
6. **Ridurre l'ambizione teorica del Corso 1**: il brief elenca molti temi (automazione dei flussi, knowledge management…) che in 8 ore diluirebbero tutto. Proponiamo: in aula solo F1–F6 + pratica intensiva; tutto il resto nel manuale, presentato in aula come "sapete dove trovarlo".
7. **Nomi e casi reali**: per la massima efficacia degli esempi (congresso specifico, sponsor tipo, provider ECM), ci serviranno alcuni riferimenti reali o verosimili dell'azienda. Possiamo lavorare con segnaposto ("Congresso Nazionale di Cardiologia 2027") finché non ce li fornisci.

---

## 9. Prossimi passi

1. Tu approvi/modifichi questa architettura (in particolare le proposte del §8)
2. Sviluppiamo i moduli F1–F6
3. Sviluppiamo il Corso 1 blocco per blocco
4. Procediamo con manuale e Corso 3 secondo l'ordine del §6

---

*Changelog: v0.1 — prima bozza dell'architettura, in attesa di approvazione.*
