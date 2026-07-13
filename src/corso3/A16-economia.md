# A16 — Economia del software: costi veri, fornitori, e la soglia del prodotto

**Corso 3 · Parte E · v1.0**

---

## 1. Le cinque voci di costo (e quella che tutti dimenticano)

Il costo di un sistema **non è il costo di costruirlo**. Con l'AI, la prima voce è crollata; le altre no — ed è per questo che *"tanto ora costruire costa poco"* è il ragionamento più pericoloso di questo decennio.

| Voce | Cosa comprende | L'errore tipico |
|---|---|---|
| **Costruzione** | Tempo tuo, servizi, fornitori | Crederla la voce principale. **Oggi è spesso la più piccola.** |
| **Esercizio** | Hosting, database, servizi terzi, modelli AI, monitoraggio | Guardare il prezzo di oggi, non quello alla scala prevista (→ A6) |
| **Manutenzione** | Aggiornamenti, bug, richieste, sicurezza, conformità | **Assumerla zero.** È la voce che uccide i progetti |
| **Opportunità** | Ciò che *non* stai facendo mentre fai questo | Non contarla mai. È spesso la più grande di tutte |
| **Dismissione** | Migrare i dati, avvisare i clienti, spegnere | Non prevederla, e restare ostaggio di ciò che si voleva abbandonare |

**La regola pratica sulla manutenzione:** per un sistema in produzione con utenti veri, **preventiva il 15–25% del costo di costruzione, ogni anno, per sempre**. Non è pessimismo: è la voce che, ignorata, trasforma un portafoglio di strumenti utili in una palude in cui tutto il tuo tempo evapora.

**Il calcolo che devi fare prima di iniziare:**
```
Vale la pena se: (valore annuo) > (esercizio annuo) + (manutenzione annua) + (opportunità)
```
e non:
```
Vale la pena se: costruirlo è veloce.
```

---

## 2. La domanda del bus

> **Se domani tu non ci fossi, chi manda avanti questo sistema?**

Per un progetto personale, la risposta "nessuno" è accettabile. **Per un prodotto con clienti, è un difetto di progettazione** — non un rischio remoto: un difetto, oggi, nel presente.

Le contromisure sono note e noiose, e nessuno le fa finché non è tardi:
1. **Decision Log aggiornato** (→ A1): la teoria del sistema esiste fuori dalla tua testa.
2. **Niente magie non spiegate.** Ogni pezzo strano ha un commento che dice *perché* è strano.
3. **Un secondo essere umano** che sa dove sono le chiavi, come si rilascia, chi chiamare.
4. **Un piano di uscita scritto**: se il sistema deve essere ceduto, ripreso o spento, cosa deve sapere chi arriva.

**Con l'AI questa domanda è più urgente, non meno.** Un sistema generato in fretta, mai letto, senza decisioni documentate, è **inaccessibile a chiunque** — incluso a te tra un anno, e incluso all'AI stessa, che non ricorda nulla di come ci siete arrivati.

---

## 3. Dirigere un fornitore tecnico

Prima o poi entrerà un professionista: per le parti critiche, per la scala, o perché il tuo tempo vale altro. **Tutta la competenza di questo corso serve soprattutto in quel momento:** saper dirigere chi ne sa più di te.

### Come si legge un preventivo

- **Un preventivo senza specifica è una fantasia**, per entrambe le parti. Se non hai specificato (→ A3), ti verrà quotato ciò che *loro* immaginano — e la differenza emergerà a metà lavoro, quando costerà di più a tutti.
- **Diffida delle stime precise su lavori lunghi.** "47 giornate" è *meno* onesto di "tra 40 e 80, e dopo il primo mese sapremo dire meglio". La precisione ostentata è un segnale di inesperienza o di malafede commerciale.
- **Chiedi cosa NON è incluso.** Il margine e i conflitti futuri stanno lì.
- **Chiedi chi possiede il codice e i dati**, e come si esce dal rapporto. **Prima di firmare.**
- **Chiedi chi scriverà materialmente il codice**, e se sarà la stessa persona tra sei mesi.

### Le cinque domande che rivelano un buon fornitore

1. *"Cosa mi state sconsigliando, tra le cose che vi ho chiesto?"* — **Chi non sconsiglia nulla non sta pensando**, sta vendendo.
2. *"Come fate a sapere che ciò che consegnate funziona?"* — Se la risposta è "lo proviamo", sai già tutto (→ A8).
3. *"Cosa succede se tra tre mesi vogliamo cambiare direzione?"*
4. *"Come gestite le migrazioni di dati e i rilasci?"* (→ A12)
5. *"Cosa fate voi con l'AI, e cosa non le fate fare?"* — La risposta ti dice se hai davanti qualcuno che ha capito il mestiere nuovo, o qualcuno che nega la realtà, o qualcuno che genera codice senza leggerlo. Tutti e tre esistono, e il terzo è il più pericoloso.

### Il committente come rischio
I progetti falliscono per colpa dei committenti almeno quanto per colpa dei fornitori: requisiti che cambiano ogni settimana, decisioni rimandate, nessun interlocutore unico, "già che ci siamo" infiniti, e l'incapacità di dire no.

> **La disciplina che pretendi, la devi per primo.**

---

## 4. La soglia del prodotto

Uno strumento interno che funziona **non è un prodotto**: è la *premessa* di un prodotto. La distanza tra i due è quasi tutta **fuori dal codice** — ed è il motivo per cui tanti strumenti interni eccellenti non diventano mai prodotti, mentre prodotti tecnicamente mediocri prosperano.

### Le sette cose che il prodotto ha e lo strumento no

1. **Utenti che non puoi istruire.** Se serve che tu spieghi, non è un prodotto: è una consulenza con del software allegato. **Il test:** un utente nuovo, senza di te, arriva al primo risultato utile? In quanto tempo?
2. **Isolamento fra clienti.** I dati di un cliente non devono poter incrociare quelli di un altro. **È una scelta architetturale da fare all'inizio**: aggiungerla dopo è una riscrittura (→ A17).
3. **Assistenza.** Qualcuno risponde quando si rompe. È un costo ricorrente, e va nel prezzo.
4. **Contratti e responsabilità.** Livelli di servizio, trattamento dei dati per conto del cliente, limiti di responsabilità. **Se tratti dati personali per conto di un cliente, sei responsabile del trattamento**: servono accordi, misure documentate, e un consulente vero.
5. **Configurabilità senza codice.** Ogni cliente vorrà una variante. **Se ogni variante è codice nuovo, non hai un prodotto: hai N progetti su misura che si spacciano per uno** — e il costo di manutenzione cresce con il quadrato dei clienti.
6. **Ciclo di vita indipendente dai clienti.** Aggiornare senza rompere chi è già dentro. Il che significa: retrocompatibilità, migrazioni, e la disciplina di non cambiare le cose per gusto.
7. **Un prezzo che regge il costo di servirli.** **Molti prodotti falliscono con clienti contenti**, perché ogni cliente costa più di quanto paga (→ A15 §5 per il caso specifico dei costi AI).

### La domanda strategica

> **Il vantaggio competitivo di un prodotto verticale non è il software: è la conoscenza del dominio incorporata nel software.**

Chiunque, oggi, può far generare un gestionale. **Nessuno che non abbia vissuto il tuo mestiere sa** cosa succede davvero quando un relatore disdice tre giorni prima, quali eccezioni ha uno statuto associativo, perché i crediti ECM si calcolano così, cosa chiede realmente uno sponsor farmaceutico.

**Il codice è diventato una commodity. Il dominio no.**

Da cui la conseguenza operativa che vale più di tutte le altre di questo corso: **ogni ora che spendi a fare ciò che chiunque può far generare è un'ora sottratta all'unica cosa che nessuno può copiarti.**

---

## 5. Costruire, comprare, o non fare (la versione economica)

Ora che costruire costa poco, la domanda *"possiamo costruirlo?"* è diventata inutile — la risposta è quasi sempre sì, ed è esattamente questo che la rende una cattiva domanda.

**Le domande utili sono tre:**
1. *Questa cosa deve esistere per i prossimi cinque anni, con tutti i suoi costi?*
2. *È qui che vive il mio vantaggio, o sto ricostruendo una commodity?*
3. *Cosa non farò, mentre faccio questo?*

**La terza è la più trascurata e la più decisiva.** Il costo di opportunità non compare in nessun preventivo, non ha una riga in nessun bilancio, e **è quasi sempre la voce più grande**.

---

## Strumenti di questo modulo

1. **Le cinque voci di costo**, con la regola del 15–25% annuo di manutenzione.
2. **La formula della convenienza**, che non contiene la velocità di costruzione.
3. **La domanda del bus** e le sue quattro contromisure.
4. **Le cinque domande al fornitore** — a partire da "cosa mi sconsigliate?".
5. **Le sette soglie del prodotto**, con l'isolamento fra clienti da decidere all'inizio.
6. **Le tre domande utili** al posto di "possiamo costruirlo?".

---

## Errori tipici

1. **Contare solo il costo di costruzione**, cioè l'unico che è crollato.
2. **Manutenzione a zero** nel piano.
3. **Ignorare il costo di opportunità.**
4. **Firmare senza sapere chi possiede codice e dati.**
5. **Chiamare "prodotto" uno strumento interno**, e scoprire le sette soglie una alla volta, sotto pressione, con il cliente che aspetta.
6. **Costruire ciò che chiunque può generare**, e trascurare l'unica cosa che nessuno può copiarti.

*v1.0 — riscrittura, fonde e sostituisce i precedenti A12, A13, A14.*
