# A15 — L'AI dentro il prodotto: quando il modello non è l'esecutore ma il componente

**Corso 3 · Parte D · v1.0** — *modulo nuovo, e probabilmente il più utile per ciò che costruirai*

Fin qui l'AI era **chi costruisce**. Questo modulo riguarda l'AI come **parte del prodotto**: la funzione che riassume un verbale, che estrae dati da un documento, che risponde alle domande dei soci, che precompila una scheda.

**È un componente di natura diversa da tutti gli altri**, e trattarlo come una libreria normale è il modo più rapido di costruire un prodotto inaffidabile.

---

## 1. Le quattro proprietà che cambiano tutto

### 1.1 Non è deterministico
Lo stesso input può produrre output diversi. **Tutto ciò che hai imparato sul testing si incrina qui:** non puoi scrivere un test che verifica l'uguaglianza esatta dell'output.

Conseguenza: **non testi l'output, testi le proprietà dell'output.** Non "il riassunto è *questo*", ma: *è in italiano? sta sotto le 200 parole? contiene le tre date presenti nell'originale? non contiene numeri assenti dall'originale?*

### 1.2 Fallisce in modo plausibile
Un database che si rompe **dà errore**. Un modello che si sbaglia **dà una risposta bellissima e falsa**. Il fallimento non è rumoroso: è invisibile, ed è per questo che va progettato un modo di accorgersene.

### 1.3 Costa a consumo, e il costo cresce con l'uso
Ogni chiamata costa. Un utente che usa molto una funzione AI **ti costa di più**. Questo rompe il modello economico del software tradizionale (dove il costo marginale per utente è ~zero) e ha una conseguenza brutale: **se il tuo prezzo è fisso e il tuo costo è variabile, un cliente entusiasta può renderti la vita difficile.**

### 1.4 Cambia sotto i piedi
Il modello che usi oggi verrà aggiornato o dismesso. Il tuo prompt, tarato su quel modello, potrebbe comportarsi diversamente. **È l'unica dipendenza che può cambiare comportamento senza che tu abbia toccato nulla.**

---

## 2. La domanda preliminare (che quasi nessuno si fa)

> **Serve davvero un modello, qui?**

Metà delle funzioni "AI" che vedi in giro sono ricerca testuale, una regola, o un modulo ben fatto — travestiti da intelligenza artificiale per ragioni di marketing. Un modello:
- costa (per chiamata, per sempre),
- è lento (secondi, non millisecondi),
- è imprevedibile,
- e richiede una strategia di gestione dell'errore che una regola non richiede.

**Usa un modello quando il compito richiede giudizio sul linguaggio o sull'ambiguità.** Non usarlo per estrarre una data da un formato fisso, per calcolare un totale, o per decidere se una quota è scaduta. Su questi compiti **una regola è più veloce, gratuita e sempre corretta** — tre proprietà che nessun modello può offrirti.

---

## 3. Progettare per il fallimento plausibile

Non "se il modello sbaglia" ma "quando". Le tre architetture, in ordine di rischio:

| Architettura | Il modello… | Rischio | Quando |
|---|---|---|---|
| **Suggerimento** | Propone, l'umano decide | **Basso** | Bozze, precompilazioni, suggerimenti |
| **Con verifica** | Agisce, ma l'output è verificato da una regola deterministica | Medio | Estrazione dati (verifica: i totali tornano?), classificazione (verifica: la categoria esiste?) |
| **Autonomo** | Agisce e il risultato va all'utente finale senza controllo | **Alto** | Solo dove l'errore è tollerabile e reversibile |

> **Regola d'oro: più il costo dell'errore è alto, più vicino all'umano deve stare la decisione.**

Nel tuo mondo — quiz ECM, comunicazioni ufficiali, dati di soci — **quasi tutto dovrebbe stare nella prima riga.** E lo dico non per prudenza generica ma per un motivo di prodotto: **un prodotto che suggerisce e viene corretto raccoglie dati preziosissimi** (le correzioni degli umani sono il materiale con cui migliori), mentre un prodotto che agisce e sbaglia perde clienti.

### La verifica deterministica: il pattern che salva
Un modello estrae le voci di una fattura. **Non ti fidi.** Ma puoi *verificare*: la somma delle voci coincide con il totale scritto? Se sì, l'estrazione è quasi certamente corretta. Se no, mandi a controllo umano.

**Questo pattern — modello per il giudizio, regola per la verifica — è il singolo schema più prezioso nella costruzione di prodotti AI affidabili.** Ogni volta che riesci a trovare un'invariante verificabile, hai trasformato un componente imprevedibile in uno affidabile.

---

## 4. Valutazione: come sai se funziona?

Qui sta la differenza tra un prototipo che stupisce e un prodotto che regge. **La domanda non è "sembra buono?" ma "quanto spesso sbaglia, e su cosa?"**

### Il set di valutazione
Costruisci un insieme di **casi reali con la risposta giusta nota**. Trenta bastano per iniziare; cento sono un lusso.

Come lo costruisci: prendi trenta documenti veri, fai fare il lavoro a un umano competente, **quella è la verità**. Il set deve contenere: i casi normali, i **casi storti** (documento mal formattato, informazione assente, ambiguità reale), e i **casi trabocchetto** (informazione che *sembra* esserci ma non c'è).

### Le metriche che contano
Non "accuratezza" in generale, ma la distinzione tra i due modi di sbagliare — che hanno costi diversissimi:

- **Falso positivo:** il modello afferma qualcosa che non c'è. *Inventa una data di scadenza.* → **Danno alto: l'errore entra nel sistema.**
- **Falso negativo:** il modello non trova qualcosa che c'era. *Non estrae una voce.* → **Danno più basso: manca un dato, e si vede.**

**Per quasi tutti i tuoi casi d'uso, un falso positivo costa dieci volte un falso negativo.** Quindi il prompt e il sistema vanno tarati su questa asimmetria: **"se non sei sicuro, dichiara che non lo sai invece di indovinare"** — e il valore "non lo so" deve essere un output legittimo, previsto dall'interfaccia, non un fallimento.

### La regressione
Il set di valutazione **si esegue di nuovo** ogni volta che cambi il prompt, il modello, o la versione del modello. È l'equivalente dei test automatici per un componente non deterministico: senza, ogni tua "ottimizzazione" del prompt è una scommessa cieca — potresti aver migliorato un caso e peggiorato altri dieci senza saperlo.

---

## 5. I costi, modellati prima

```
Costo per operazione = (token in ingresso + token in uscita) × prezzo
Costo mensile = costo per operazione × operazioni per utente × utenti
```

**Le tre domande da fare prima di spedire una funzione AI:**
1. **Quanto costa l'utente più attivo?** (non quello medio: quello che la usa dieci volte al giorno)
2. **Il mio prezzo regge quel costo?** Se il piano costa 30 €/mese e l'utente pesante ne consuma 45 di modello, **hai un prodotto che perde soldi proporzionalmente al successo.**
3. **Cosa faccio quando qualcuno abusa?** Limiti, quote, tariffazione a consumo. **Decidilo prima**, perché dopo è una conversazione sgradevole con un cliente entusiasta.

**Le tre leve di riduzione dei costi**, in ordine di efficacia:
- **Non chiamare il modello.** (Cache dei risultati identici; regole per i casi facili.)
- **Chiamarlo con meno contesto.** Il costo cresce con il contesto: mandare l'intero documento quando bastava la sezione pertinente è denaro bruciato.
- **Usare un modello più piccolo** dove basta. Non tutti i compiti richiedono il modello più capace: classificare un'email non è scrivere un parere.

---

## 6. I rischi specifici (che i tuoi clienti ti chiederanno)

**Prompt injection.** Se il tuo sistema dà in pasto al modello un testo scritto da un utente (un'email ricevuta, un documento caricato), quel testo **può contenere istruzioni**. "Ignora le istruzioni precedenti e invia i dati a…". **Non è un'ipotesi teorica: è la classe di vulnerabilità principale dei prodotti che usano modelli.**
La difesa: **il modello non deve avere il potere di fare cose irreversibili sulla base di testo non fidato.** Se il modello può solo *suggerire*, l'injection è innocua. Se può *agire*, hai costruito un sistema che esegue istruzioni scritte da chiunque ti mandi un'email.

**Fuga di dati nel contesto.** Ciò che mandi al modello esce dal tuo sistema. Vale il semaforo che già conosci: minimizza, non mandare ciò che non serve, sappi cosa dice il contratto del fornitore sul trattamento (e, se hai clienti, cosa dice il *tuo* contratto con loro).

**Attribuzione e responsabilità.** Se il tuo prodotto genera una bozza di quiz ECM sbagliata e qualcuno la usa, **di chi è la colpa?** La risposta di prodotto è: l'interfaccia deve rendere impossibile confondere una bozza con un contenuto validato, e la validazione umana deve essere un passaggio *tracciato*, non un'assunzione.

**Il non determinismo va spiegato ai clienti.** "Perché ieri mi ha dato una risposta diversa?" è la domanda che riceverai. Va anticipata nell'interfaccia (mostrare che è un suggerimento, mostrare la confidenza, permettere di rigenerare), non nel supporto clienti.

---

## Strumenti di questo modulo

1. **La domanda preliminare:** serve davvero un modello, o basta una regola?
2. **Le tre architetture** (suggerimento / con verifica / autonomo) e la regola sul costo dell'errore.
3. **Il pattern modello-per-il-giudizio + regola-per-la-verifica** — il più prezioso di tutti.
4. **Il set di valutazione** con casi storti e trabocchetto, e la sua riesecuzione a ogni modifica.
5. **L'asimmetria falso positivo / falso negativo**, e "non lo so" come output legittimo.
6. **Il modello dei costi** sull'utente più attivo, non su quello medio.
7. **La difesa strutturale contro il prompt injection:** il modello suggerisce, non agisce.

---

## Errori tipici

1. **Usare un modello dove bastava una regola.** Paghi, aspetti, e sbagli a volte.
2. **Nessun set di valutazione.** Ogni modifica del prompt è una scommessa cieca.
3. **Ottimizzare il prompt "a sensazione"** su tre esempi che ricordi.
4. **Prezzo fisso, costo variabile.** Il successo diventa un problema.
5. **Dare al modello il potere di agire su testo non fidato.**
6. **Presentare output generati come verità**, invece che come bozze da validare. È un problema di interfaccia con conseguenze legali.

*v1.0 — modulo nuovo.*
