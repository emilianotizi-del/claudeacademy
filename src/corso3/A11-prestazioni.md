# A11 — Prestazioni e scala: numeri, non paure

**Corso 3 · Parte C · v1.0** — *modulo nuovo*

Le prestazioni sono il territorio dove si spreca più tempo su problemi che non esistono, mentre si ignorano quelli che uccidono. La disciplina è una sola:

> **Nessuna decisione sulle prestazioni senza un numero. Mai.**

---

## 1. Le tre leggi che devi conoscere

### 1.1 «L'ottimizzazione prematura è la radice di ogni male»
La frase di Knuth è la più citata e la più fraintesa dell'informatica. La citazione completa dice: *"dovremmo dimenticare le piccole efficienze, diciamo il 97% delle volte"* — **ma il 3% restante è decisivo**, e va identificato **misurando**.

Il senso corretto: non è "non pensare mai alle prestazioni". È **"non indovinare dove sta il problema"**. Le decisioni strutturali (il modello dati, gli accessi al database) vanno pensate subito, perché sono irreversibili. Le micro-ottimizzazioni vanno rimandate finché un profilo non ti dice dove guardare.

### 1.2 La latenza non è la banda
Due cose diverse che tutti confondono:
- **Latenza** = quanto ci mette *una* operazione. Migliorabile solo facendo *meno cose*.
- **Banda/throughput** = quante operazioni al secondo. Migliorabile aggiungendo macchine.

Aggiungere server **non rende più veloce una singola richiesta lenta**. Se una pagina impiega 4 secondi, raddoppiare i server ti darà due pagine lente in parallelo.

### 1.3 La legge di Amdahl, in versione utile
Se una parte del sistema consuma il 90% del tempo, ottimizzare **tutto il resto** all'infinito ti dà al massimo il 10% di miglioramento. **Quindi:** trova la parte che consuma, ignora le altre. Il che richiede di misurare, il che ci riporta al punto 1.

---

## 2. Il problema che avrai davvero: N+1

In un sistema gestionale, il 90% dei problemi di prestazione ha **una sola causa**, e ha un nome.

**Il caso.** Devi mostrare l'elenco di 200 soci con l'anno dell'ultima quota pagata. Il codice generato, che è pulitissimo e leggibile:
1. fa una query per prendere i 200 soci;
2. **per ciascuno**, fa una query per prendere le sue quote.

Totale: **201 query**. In locale, con 20 soci di prova, gira in 40 millisecondi e sembra perfetto. In produzione, con 5.000 soci e la rete di mezzo, sono **5.001 query** e la pagina impiega venti secondi. Il codice non è cambiato. È cambiata la realtà.

**Perché l'AI lo genera:** perché è il codice più naturale, più leggibile, e perfettamente corretto. **La correttezza non implica la sostenibilità.**

**Il segnale d'allarme, in una regola:**

> **Ogni volta che vedi una query dentro un ciclo, hai un N+1.**

**La difesa strutturale:** un contatore di query per richiesta, con un allarme che scatta oltre una soglia (es. 20). Costa mezz'ora, e ti avvisa il giorno in cui il problema nasce — non sei mesi dopo, quando un cliente si lamenta.

**Il prompt:**
```
Analizza questo codice per problemi N+1: mostrami ogni punto in cui viene
eseguita una query dentro un ciclo, o in cui il numero di query cresce con
il numero di risultati. Per ciascuno, riscrivilo come query singola e dimmi
quante query risparmia con 1.000 record.
```

---

## 3. Gli altri quattro assassini

**Query senza indice.** Il database cerca riga per riga. Su 1.000 record è impercettibile; su 500.000 è un minuto. **Ogni colonna su cui filtri o ordini frequentemente ha bisogno di un indice** — e ogni indice rallenta le scritture, quindi non se ne mettono "per sicurezza": si misurano.

**Caricare tutto in memoria.** "Prendi tutti gli iscritti e filtra nel codice" funziona con 100 record e uccide il server con 100.000. Si filtra nel database, sempre.

**Lavoro sincrono che poteva essere differito.** L'utente clicca "invia comunicazione a tutti", il sistema invia 400 email **mentre lui aspetta**, la richiesta va in timeout, lui riclicca, e ora sono 800 email. Le operazioni lunghe si mettono in **coda**: il sistema risponde subito "in corso", e lavora dietro. (Ed è idempotente, → A2.)

**Payload obesi.** L'API restituisce l'intero oggetto con tutte le relazioni quando alla pagina servivano tre campi. Sulla rete di un telefono in un centro congressi, questo è la differenza tra usabile e inservibile.

---

## 4. Il budget delle prestazioni

Si definisce **prima**, come vincolo non funzionale della specifica (→ A3), non "dopo, se sarà lento":

```
NF-perf-1 · Elenco iscritti (fino a 1.000): risposta < 500 ms al 95° percentile
NF-perf-2 · Ricerca socio: < 200 ms
NF-perf-3 · Importazione di 5.000 record: < 60 s, in background, con avanzamento
NF-perf-4 · Nessuna richiesta esegue più di 20 query al database
```

**Il 95° percentile, non la media.** La media nasconde i disastri: se 95 richieste su 100 impiegano 100 ms e 5 impiegano 30 secondi, la media è 1,6 secondi e **sembra accettabile** — mentre il 5% dei tuoi utenti sta guardando una schermata bloccata. Le medie mentono; i percentili no.

---

## 5. Scalare: i tre assi, in ordine di convenienza

| Asse | Cosa significa | Quando |
|---|---|---|
| **1 · Fare meno lavoro** | Query migliori, meno dati, meno chiamate | **Sempre per primo.** È gratis e migliora tutto |
| **2 · Fare il lavoro dopo** | Code, elaborazione in background, calcoli notturni | Quando il lavoro non deve essere immediato |
| **3 · Fare il lavoro con più macchine** | Più server, più repliche | **Ultimo.** Costa soldi ogni mese, per sempre |

**L'errore quasi universale è saltare al terzo.** Aggiungere server è la soluzione più visibile, più costosa, e quella che **nasconde il problema invece di risolverlo**: la query N+1 continua a esserci, adesso semplicemente la paghi in bolletta ogni mese, per sempre.

### Sulla cache
La cache è tentante e **crea una seconda verità** (→ A2). Prima di aggiungerne una:
1. *Ho misurato la lentezza che sto curando?*
2. *Cosa succede quando il dato cambia? Chi invalida la cache?*
3. *Cosa succede se l'utente vede un dato vecchio di due minuti?* Su un elenco di sessioni: nulla. Su "risulto in regola con la quota?": un problema.

> Come dice la battuta più vera dell'informatica, ci sono due problemi difficili: **l'invalidazione della cache**, dare i nomi alle cose, e gli errori off-by-one.

---

## 6. Misurare: il minimo indispensabile

**Prima di ottimizzare**, tre strumenti:
1. **Il tempo di ogni richiesta**, registrato. Senza, stai indovinando.
2. **Il conteggio delle query** per richiesta. Trova gli N+1 da solo.
3. **Il piano di esecuzione** della query lenta (`EXPLAIN ANALYZE`). Ti dice se il database sta usando l'indice o sta leggendo tutto. Non devi saperlo interpretare: **chiedilo al modello, incollandogli l'output**. Lì è bravissimo, e lì i dati ci sono.

**Il test che vale più di tutti:** *popola il database di prova con dieci volte i dati che ti aspetti, e usa il sistema.* Dieci minuti di lavoro, e scopri in anticipo tutto ciò che ti si romperebbe addosso tra un anno — quando avrai clienti veri e nessun margine.

---

## Strumenti di questo modulo

1. **La regola:** nessuna decisione sulle prestazioni senza un numero.
2. **Il riconoscimento dell'N+1** — query dentro un ciclo — e il prompt che lo caccia.
3. **Il budget delle prestazioni** nella specifica, espresso in percentili.
4. **I tre assi della scala**, con "più macchine" all'ultimo posto.
5. **Il contatore di query** con allarme, come difesa strutturale.
6. **Il test dei dati 10x**, prima che sia la realtà a farlo.

---

## Errori tipici

1. **Ottimizzare a naso.** Quasi sempre si ottimizza la parte sbagliata.
2. **Testare su dati giocattolo.** Tutti i problemi di scala sono invisibili con 20 record.
3. **Ragionare per medie.** Nascondono esattamente gli utenti che stai perdendo.
4. **Aggiungere server** invece di togliere query.
5. **Cache senza strategia di invalidazione.** Hai barattato la lentezza con l'incoerenza — e l'incoerenza è peggio, perché non si vede.
6. **Indici "per sicurezza".** Rallentano le scritture e occupano spazio: si misurano.

*v1.0 — modulo nuovo.*
