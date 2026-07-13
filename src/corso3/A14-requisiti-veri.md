# A14 — I requisiti veri: cosa la gente ti dice, cosa la gente fa

**Corso 3 · Parte D · v1.0** — *modulo nuovo*

Il modo più efficiente di costruire la cosa sbagliata è **chiedere alle persone cosa vogliono e dargliela**. Questo modulo è su come si scopre ciò che serve davvero — competenza che l'AI non ti dà, e che vale più di tutte le altre messe insieme, perché determina *se* stai costruendo la cosa giusta, mentre tutto il resto determina solo *quanto bene*.

---

## 1. Perché le richieste degli utenti sono inaffidabili

Non perché le persone mentano. Per tre ragioni strutturali:

**1 · Descrivono la soluzione che immaginano, non il problema che hanno.** "Mi serve un pulsante per esportare in Excel" — perché? Perché ogni mese deve mandare un elenco al tesoriere. Il problema è l'invio mensile, non il pulsante. La soluzione giusta potrebbe essere un invio automatico, e non ci pensava perché non sapeva fosse possibile.

**2 · Non sanno cosa fanno.** L'introspezione sul proprio lavoro è notoriamente inaffidabile: la gente racconta il processo *ideale*, non quello reale — dimentica i workaround, le eccezioni, il post-it sul monitor, il "poi però in quel caso chiamo Marco". **Ed è proprio lì che vive il valore.**

**3 · Chiedono l'incrementale.** Un cavallo più veloce. Se avessero saputo immaginare l'automobile, l'avrebbero già chiesta. **Il salto lo devi vedere tu**, che conosci sia il loro dominio sia ciò che è possibile — ed è una combinazione rara, ed è la tua.

---

## 2. Le cinque tecniche che funzionano

### 2.1 Guardali lavorare (la più potente, di gran lunga)
Siediti accanto alla persona **mentre fa il lavoro vero**, in silenzio, e guarda. Non intervistarla: **guardala**.

Vedrai cose che nessuna intervista avrebbe prodotto: il file Excel di appoggio che nessuno ha mai menzionato; il copia-incolla da tre sistemi diversi; il campo "note" usato per contenere informazioni strutturate che il sistema non prevedeva; la telefonata che risolve in 30 secondi ciò che il software non permette.

> **Il campo "note" pieno di informazione strutturata è la firma di un requisito mancato.** È lì che gli utenti scrivono ciò che il tuo modello dati non sa rappresentare, ed è la miniera più ricca che troverai.

### 2.2 Chiedi "raccontami l'ultima volta"
Non "come fai di solito?" (produce il processo ideale), ma:

> *"Raccontami l'ultima volta che hai dovuto organizzare i viaggi della faculty. Passo per passo. Cosa è andato storto?"*

Gli **episodi concreti** hanno dettagli veri; le **generalizzazioni** sono già filtrate e ripulite. Questa singola sostituzione — dall'astratto all'episodico — è la tecnica che raddoppia la qualità di ogni intervista.

### 2.3 I cinque perché
Ogni richiesta ha una richiesta sotto:
> *"Serve un campo per la data di scadenza del passaporto."*
> — Perché? *"Per sapere se un relatore straniero può viaggiare."*
> — Perché ti serve saperlo? *"Perché una volta è saltato tutto tre giorni prima."*
> — Cosa hai fatto? *"Ho chiamato tutti uno per uno, un mese prima, per controllare."*
> — **Il requisito vero non è un campo: è un promemoria automatico** un mese prima per i relatori esteri. E in più: **non devi tenere il numero di passaporto** — cioè non devi assumerti il passivo di un dato sensibile (→ A9). Il requisito superficiale ti avrebbe fatto costruire una cosa peggiore *e* più rischiosa.

### 2.4 Il "no" come strumento diagnostico
Quando qualcuno chiede una funzionalità, rispondi: *"e se non la facessimo?"* Le risposte utili sono tre:
- *"Allora continuo a farlo a mano, ci metto due minuti"* → **non è un requisito.**
- *"Allora non posso usare il sistema"* → **è un requisito bloccante.**
- *"Allora dovrei ricordarmene ogni volta e prima o poi sbaglio"* → **è il requisito vero**: non la funzione, ma la protezione dall'errore.

### 2.5 Il prototipo come domanda
Le persone non sanno dirti cosa vogliono, ma **sanno riconoscerlo quando lo vedono**, e sono bravissime a dirti cosa non va. Un prototipo grezzo mostrato in cinque minuti produce più informazione di due ore di riunione. **Con l'AI il prototipo costa un pomeriggio: usa questa leva, è la più grande che ti ha dato.**

---

## 3. La distinzione che salva i prodotti

| | **Requisito** | **Desiderio** |
|---|---|---|
| Origine | Un problema che accade davvero, con una frequenza | "Sarebbe bello se…" |
| Test | Se non c'è, qualcosa si rompe o costa | Se non c'è, non se ne accorge nessuno |
| Chi lo chiede | Chi il lavoro lo fa | Chiunque, in riunione |
| Come si verifica | **Guardando cosa fa oggi** al posto di questa cosa | Non si verifica |

**Il test definitivo:** se la funzionalità non esistesse, **cosa farebbe l'utente al posto suo?** Se la risposta è "una cosa che gli costa venti minuti a settimana", hai un requisito quantificato. Se è "non lo so, niente", hai un desiderio, e i desideri **si scrivono nel fuori-scopo** (→ A3).

---

## 4. La trappola del cliente unico

Il tuo primo cliente ti chiederà cose che valgono **solo per lui**. Se le costruisci tutte, non hai un prodotto: hai **un progetto su misura mascherato da prodotto**, e il secondo cliente lo scoprirà (→ A17).

**Il criterio, prima di accettare una richiesta:**

> **"Questa cosa la vorrebbero anche gli altri, o è la sua eccezione?"**

Se è la sua eccezione, hai tre strade — in ordine di preferenza:
1. **Rendila configurabile** (una regola che si accende, non codice nuovo).
2. **Digli di no**, e spiega perché. Un no motivato non perde clienti; un prodotto ingestibile sì.
3. **Fagliela pagare come personalizzazione**, sapendo che diventa manutenzione tua per sempre — e mettendolo nel prezzo.

**La quarta strada — dire di sì gratis "tanto è una cosa piccola" — è quella che uccide i prodotti.** Non con un colpo solo: con quaranta piccole cose che, insieme, rendono ogni futuro cambiamento impossibile.

---

## 5. Misurare l'uso, non l'entusiasmo

**Le persone sono gentili.** Ti diranno che il sistema è bello e utile, e poi continueranno a usare l'Excel. Non stanno mentendo: **stanno evitando un conflitto**, che è una cosa che gli esseri umani fanno benissimo.

**L'unico dato che conta è il comportamento:**
- Lo aprono? Quanto spesso? **Da soli, o solo quando glielo chiedi tu?**
- Hanno smesso di fare la cosa di prima? (Se l'Excel è ancora lì, non hai vinto: hai aggiunto lavoro.)
- Dove si fermano? Dove abbandonano a metà?
- Cosa scrivono nel campo "note"? (→ §2.1)

**Il segnale definitivo di successo di uno strumento interno:** *qualcuno si arrabbia quando non funziona.* Finché nessuno si arrabbia, nessuno ci contava davvero — e quindi non stava risolvendo nessun problema vero.

---

## 6. Il prompt per estrarre requisiti da un'osservazione

Dopo aver guardato qualcuno lavorare, e preso appunti grezzi:

```
Ecco gli appunti di un'osservazione sul campo: [appunti].

1. Elenca i problemi REALI che ho osservato (non le soluzioni che la persona
   ha proposto).
2. Per ciascuno: con che frequenza accade, quanto costa quando accade, cosa
   fa oggi la persona per sopravvivere (il workaround).
3. Identifica i workaround: sono la prova di un requisito non soddisfatto.
   Cosa rivela ciascuno?
4. Quali informazioni la persona sta gestendo in modo informale (a memoria,
   su carta, nel campo note, in un Excel di appoggio)? Quelle sono entità
   che il modello dati dovrebbe rappresentare.
5. Distingui: cosa è un problema di questa persona, cosa sarebbe un problema
   di chiunque faccia questo lavoro.
6. Cosa NON dovrei costruire, tra le cose che ha chiesto esplicitamente?
```

Il punto 4 è quello che trova le entità mancanti del modello dati; il punto 6 è quello che protegge lo scopo.

---

## Strumenti di questo modulo

1. **L'osservazione sul campo** — e il campo "note" come firma di un requisito mancato.
2. **"Raccontami l'ultima volta"** invece di "come fai di solito".
3. **I cinque perché**, che spesso rivelano un requisito *diverso e migliore* di quello chiesto.
4. **Il "no" diagnostico**: "e se non lo facessimo?"
5. **Il test requisito/desiderio:** cosa farebbe l'utente al posto suo?
6. **Il criterio del cliente unico**: configurabile, oppure no, oppure a pagamento — mai "sì, gratis, tanto è piccola".
7. **Il prompt di estrazione requisiti dall'osservazione.**

---

## Errori tipici

1. **Costruire la soluzione che ti chiedono**, invece di risolvere il problema che hanno.
2. **Intervistare invece di guardare.**
3. **Fidarsi dell'entusiasmo.** L'unica prova è che abbiano smesso di fare la cosa di prima.
4. **Dire sì a tutto al primo cliente**, e ritrovarsi con un su misura che non scala.
5. **Ignorare i workaround.** Sono la mappa dei requisiti, scritta dagli utenti stessi, gratis.
6. **Confondere "nessuno si lamenta" con "funziona".** Nessuno si lamenta anche di ciò che nessuno usa.

*v1.0 — modulo nuovo.*
