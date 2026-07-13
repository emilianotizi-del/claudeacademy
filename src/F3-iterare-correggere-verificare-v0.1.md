# F3 — Iterare, correggere, verificare

**Codice modulo:** F3 · **Versione:** 0.1 · **Data:** 13/07/2026
**Dipendenze:** F1 (allucinazioni), F2 (briefing), DATI-01 v1.0
**Richiamato da:** F4, F5 · Corso 1 blocchi 1.3 e 1.5 · Manuale M3
**Etichette:** [FATTO] documentato · [BP] best practice consolidata · [OPINIONE] dibattuta · [SUGG] suggerimento del team

---

## 1. Scopo del modulo

Al termine di questo modulo saprai:

- **correggere senza ripartire da zero**: guidare Claude verso il risultato con ritocchi mirati;
- **ottenere critiche utili** invece di conferme compiacenti;
- applicare un **protocollo di verifica** che intercetta le allucinazioni prima che escano dall'azienda.

Prerequisiti: → F1 (perché Claude sbaglia con sicurezza) e → F2 (il briefing).

---

## 2. Il principio

> **La prima risposta è una bozza. Il tuo lavoro comincia lì.**

**[BP]** Lavorare con Claude non è "domanda → risposta → fine": è una conversazione in cui raffini, correggi e verifichi. Chi si ferma alla prima risposta usa il 30% dello strumento; chi impara a iterare usa il resto.

---

## 3. Correggere senza ripartire

**[BP]** La correzione più efficiente è **mirata e conservativa**: di' cosa tenere e cosa cambiare, invece di riscrivere la richiesta.

Frasi che funzionano (da adattare, non formule magiche):

- **Correzione chirurgica** — "Tieni tutto, cambia solo il secondo paragrafo: troppo formale."
- **Vincolo aggiunto** — "Più corto: massimo 100 parole, togli i convenevoli."
- **Cambio di direzione** — "Meno tecnico: la legge anche chi non conosce l'ECM."
- **Riformulazione** — "Non era questo che intendevo: mi serve un sollecito, non una richiesta di informazioni."
- **Varianti locali** — "Dammi tre versioni alternative solo della frase di chiusura."
- **Riorganizzazione** — "Contenuto giusto, ordine sbagliato: prima le date, poi la logistica."
- **Trasparenza** — "Spiegami perché hai scelto questo tono, prima di cambiarlo."
- **Marcia indietro** — "La versione di due messaggi fa era meglio: riparti da quella."

**Esempio B Best.** Bozza di comunicazione alla faculty di Romanestesia: la prima versione è corretta ma fredda. "Tieni la struttura; rendi il primo paragrafo più caloroso: molti di loro sono relatori storici, e aggiungi una riga di ringraziamento per l'edizione 2025" produce in un passaggio ciò che una riscrittura da zero otterrebbe in tre.

### 3.1 Quando invece ripartire da capo

**[BP]** Iterare non è sempre la scelta giusta. Segnali che conviene una chat nuova (o una richiesta riscritta):

1. **La risposta è fuori strada** sul fondamentale, non sul dettaglio: correggere un fraintendimento di fondo costa più che ribriefare bene.
2. **Ti sei accorto che il briefing era sbagliato**: se l'errore è nel tuo prompt, correggi il prompt, non la risposta.
3. **La conversazione si è "sporcata"**: dopo molte correzioni contraddittorie, Claude trascina i detriti dei tentativi precedenti. Il contesto inquinato è tema di → F4.

**[SUGG] La regola delle tre correzioni**: se dopo tre correzioni mirate sullo stesso punto il risultato non va, smetti di correggere. Riformula la richiesta da zero incorporando tutto ciò che hai imparato sugli errori — spesso in una chat nuova.

---

## 4. Ottenere critiche utili

Claude tende ad assecondare (→ F1 §4.4). Per farlo lavorare da revisore serve chiederlo esplicitamente. Tre modalità, in ordine di struttura crescente:

1. **L'avvocato del diavolo** — "Fai l'avvocato del diavolo: cosa obietterebbe un anestesista esperto a questo programma? E cosa obietterebbe lo sponsor?"
2. **Il revisore con checklist** — "Rivedi questa mail rispetto a questi criteri: (a) chiarezza della richiesta, (b) tono adeguato a un primario, (c) date e riferimenti corretti, (d) lunghezza. Per ogni criterio: voto e motivazione."
3. **Il confronto tra opzioni** — "Ecco due versioni del regolamento espositori. Confrontale punto per punto e dimmi quale sceglieresti per un congresso da 300 partecipanti, e perché."

**[SUGG]** Il trucco che li potenzia tutti: chiedi la critica **prima** di dire quale versione preferisci tu. Se dichiari la tua preferenza, la critica tenderà a confermarla.

---

## 5. Verificare: il protocollo anti-allucinazione

Da → F1 sai *dove* Claude sbaglia (nomi, date, numeri, norme, citazioni). Qui c'è *come* difendersi, in quattro passi:

1. **Individua i fatti sensibili.** Prima di usare un output, evidenzia tutto ciò che è verificabile: ogni nome, data, numero, riferimento normativo, citazione.
2. **Distingui la provenienza.** Chiedi: "Per ciascuna di queste informazioni: viene dal documento che ti ho fornito o dalla tua conoscenza generale?" **[BP]** Ciò che viene dal *tuo* documento si controlla sul documento; ciò che viene dalla "conoscenza generale" è a rischio allucinazione e va verificato fuori.
3. **Controlla sulla fonte primaria.** Norme ECM → documento Agenas ufficiale; dati faculty → la vostra anagrafica; citazioni → l'articolo originale. Se Claude ha usato la ricerca web, apri il link e verifica che dica davvero ciò che è stato riportato.
4. **In caso di dubbio, triangola.** Fai la stessa domanda in una chat nuova, senza suggerire la risposta: se le due risposte divergono, nessuna delle due è affidabile senza fonte.

Tecniche complementari:

- **Far dichiarare l'incertezza** — "Dimmi esplicitamente quali di queste informazioni non sei sicuro siano corrette." **[OPINIONE]** utile ma non infallibile: l'autovalutazione dei modelli è imperfetta; trattala come un indizio, non come una garanzia.
- **Verifica asimmetrica** **[SUGG]** — proporziona il controllo al danno: una data in una mail interna vale un'occhiata; una scadenza in una comunicazione ECM ufficiale vale la fonte primaria, sempre.

---

## 6. Il ciclo completo su un caso reale

**Compito:** dal verbale grezzo della riunione con Getinge alla sintesi da inviare alla direzione.

1. **Briefing** (→ F2): incolli il verbale, chiedi la sintesi nel formato standard (decisioni / azioni con responsabile e scadenza / punti rinviati).
2. **Prima bozza**: struttura giusta, ma le azioni non hanno tutte una scadenza.
3. **Correzione mirata**: "Tieni tutto; per le azioni senza scadenza, segna [DA DEFINIRE] invece di ometterle."
4. **Revisione critica**: "Rileggi il verbale: c'è qualche decisione presente nel testo che la sintesi non riporta?" — emerge un punto sull'allestimento dello spazio espositivo sfuggito alla prima passata.
5. **Verifica**: le tre date e l'importo citati si controllano sul verbale originale (provengono dal documento fornito → controllo interno, un minuto).
6. **Chiusura**: la sintesi parte con la tua firma — e la responsabilità è tua, non di Claude (→ F6).

---

## 7. Errori tipici

1. **Accanirsi**: dieci correzioni sullo stesso punto invece di applicare la regola delle tre correzioni.
2. **Correggere a mano ciò che Claude correggerebbe meglio** — o il contrario: ritoccare tu una parola è più rapido che spiegarlo; rifare tu l'intera struttura non lo è.
3. **"Si è corretto, quindi ora è vero"**: una risposta corretta dopo la tua obiezione non è più affidabile della prima — Claude potrebbe semplicemente averti assecondato. Vale il protocollo del §5.
4. **Chiedere la critica dopo aver dichiarato la propria preferenza** (§4).
5. **Verificare tutto o niente**: il controllo indiscriminato non è sostenibile e quello assente è pericoloso — usa la verifica asimmetrica.
6. **Chat infinite**: correzioni su correzioni per giorni nella stessa conversazione — il contesto si inquina (→ F4).

---

## 8. Esercizi (blocco 1.3)

- Dal verbale fornito in aula: sintesi in formato standard, poi **due correzioni mirate** (una di tono, una di contenuto) senza mai riscrivere la richiesta.
- Sulla bozza risultante: **una revisione critica** con checklist a quattro criteri, definita da te.
- Caccia all'allucinazione (blocco 1.5): in un output preparato dal docente, individuare i fatti sensibili, chiederne la provenienza e classificare cosa va verificato dove.

---

## 9. Punti chiave del modulo

1. La prima risposta è una bozza: il lavoro comincia lì.
2. Correggi in modo mirato: di' cosa tenere, non solo cosa cambiare.
3. Regola delle tre correzioni: poi si riformula (spesso in chat nuova).
4. La critica va chiesta esplicitamente — e prima di dichiarare la tua preferenza.
5. Protocollo anti-allucinazione: individua i fatti → chiedi la provenienza → fonte primaria → triangola.
6. "Si è corretto" non significa "ora è vero".
7. Verifica asimmetrica: proporziona il controllo al danno potenziale.
8. Contesto che si sporca? È il tema di → F4.

---

## 10. Collegamenti

- **← F1** (allucinazioni: dove) · **← F2** (il briefing) · **→ F4** (contesto e chat nuove) · **→ F6** (responsabilità sull'output)
- **Corso 1**: blocchi 1.3 e 1.5 · **Manuale**: M3

---

*Changelog: v0.1 — prima stesura; caso Getinge e faculty da DATI-01 v1.0.*
