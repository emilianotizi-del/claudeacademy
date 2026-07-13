# A9 — Sicurezza: pensare come chi vuole entrare

**Corso 3 · Parte C · v1.0**

La sicurezza non è una funzionalità che si aggiunge: è una **proprietà del sistema**, e come tutte le proprietà si progetta all'inizio o non c'è. Non devi saperla implementare. Devi saper **fare le domande giuste, riconoscere le risposte evasive, e sapere dove ti stanno mentendo — incluso quando a mentirti è un modello che ha dimenticato un controllo.**

Ricorda la categoria 1.5 di A8: **l'AI non aggiunge vulnerabilità, le dimentica.** Il codice sembra corretto, funziona in ogni prova, e manca il controllo che nessuno vede.

---

## 1. Il modello di minaccia in quattro domande

### 1.1 Cosa proteggo?
Non "il sistema": **le cose specifiche**. I dati personali dei soci. L'integrità di una delibera. La disponibilità del servizio il giorno del congresso. La tua reputazione con un cliente istituzionale. Sono beni diversi, con avversari e contromisure diverse.

### 1.2 Da chi?
Elencare gli avversari cambia radicalmente le priorità:

| Avversario | Motivazione | Capacità | Contromisura principale |
|---|---|---|---|
| **Curioso interno** (un socio) | Guardare dati altrui per curiosità | Bassa: prova a cambiare un numero nell'URL | **Controllo di autorizzazione su ogni accesso** |
| **Utente scontento** | Danneggiare, esporre | Media | Log, tracciabilità, permessi minimi |
| **Bot automatico** | Credenziali, risorse di calcolo, spam | Alta ma indiscriminata | Aggiornamenti, niente segreti esposti, limiti di frequenza |
| **Attaccante mirato** | Dati di valore, riscatto | Alta e paziente | Difesa in profondità, monitoraggio, backup isolati |
| **Tu stesso** | Errore, stanchezza | Massima (hai tutti i permessi) | Ambienti separati, permessi minimi anche per te, backup |

**L'ultimo è statisticamente il più pericoloso.** La maggioranza degli incidenti in sistemi piccoli non è un attacco: è una cancellazione accidentale, una configurazione sbagliata, un segreto pubblicato per distrazione.

### 1.3 Quali sono le vie d'ingresso?
Autenticazione · input degli utenti · file caricati · dipendenze di terze parti (il codice che non hai scritto *né tu né l'AI*: quello di altri, che aggiorni ciecamente) · le API · **l'amministratore** · il fornitore cloud · i backup (spesso protetti peggio dei dati vivi, ed equivalenti).

### 1.4 Cosa succede quando — non se — accade?
Rilevazione, contenimento, notifica, ripristino. **Un sistema che non può accorgersi di essere stato violato è, in senso operativo, già violato**: la domanda "siamo stati compromessi?" non ha risposta, e questa è la peggiore delle risposte.

Sotto GDPR, una violazione di dati personali va **notificata all'autorità entro 72 ore**. Non puoi notificare ciò che non sai. I log non sono un lusso: sono la condizione per adempiere.

---

## 2. I sei principi che non cambiano mai

1. **Privilegio minimo.** Ognuno — utente, servizio, token, e tu — ha il minimo necessario. Il token che serve a leggere non deve poter cancellare.
2. **Difesa in profondità.** Nessun controllo singolo deve essere l'unico ostacolo. Se l'interfaccia nasconde un pulsante, il server deve comunque rifiutare la richiesta.
3. **Non fidarsi dell'input.** Ogni dato che arriva da fuori è ostile finché non è validato — **anche quello che arriva dal tuo stesso frontend**, perché chiunque può parlare direttamente al tuo server ignorando il frontend. Questo è il punto che i non tecnici sbagliano più spesso: *i controlli nell'interfaccia non sono controlli di sicurezza, sono cortesie.*
4. **Sicuro per default.** L'impostazione predefinita è chiusa; l'apertura è una decisione esplicita, tracciata.
5. **Non inventare crittografia né autenticazione.** L'unico dominio in cui "l'ho fatto io" è quasi sempre la risposta sbagliata. Si usano librerie standard e fornitori seri (→ A6).
6. **Il dato che non hai raccolto non può essere rubato.** La minimizzazione non è un adempimento burocratico: **è la contromisura di sicurezza più efficace che esista**, perché elimina il bene invece di difenderlo.

---

## 3. Le cinque falle che troverai davvero

Non ti servono trenta categorie teoriche. In un sistema come i tuoi, il 90% del rischio reale sta qui.

### 3.1 Autorizzazione mancante o incompleta (la n.1 assoluta)
Il sistema verifica **chi sei** (autenticazione) ma non **cosa puoi vedere** (autorizzazione). Il caso classico, banale e devastante:

> L'URL è `/iscritti/1042`. L'utente cambia il numero in `1043`. E vede i dati di un altro.

Questa singola falla — cambiare un identificativo per accedere a dati altrui — è **la vulnerabilità più diffusa nelle applicazioni web**, e l'AI la introduce costantemente per omissione: genera una query che recupera l'iscritto per `id`, e dimentica di aggiungere *"…e verifica che appartenga all'organizzazione dell'utente corrente."*

**La difesa strutturale:** l'autorizzazione **non deve essere un controllo che si può dimenticare**. Va imposta nello strato dei dati — in Postgres, per esempio, con politiche di accesso a livello di riga — così che una query che dimentica il filtro **non restituisca nulla** invece di restituire tutto. È la differenza tra un sistema che perdona gli errori e uno che li amplifica.

**Il test che devi pretendere:** *creo due organizzazioni, faccio login come utente della prima, e provo ad accedere a ogni singola risorsa della seconda, per identificativo. Deve fallire ovunque, sempre.* Questo test si scrive una volta e si esegue per sempre.

### 3.2 Injection (SQL e affini)
Input dell'utente che finisce dentro una query e ne cambia il significato. **La difesa è nota da vent'anni** (query parametrizzate) e le librerie moderne la applicano da sole — l'AI raramente sbaglia qui. Diventa un rischio quando qualcuno costruisce query "concatenando stringhe" per fare qualcosa di dinamico: **è quello il momento in cui devi fermare tutto e chiedere.**

### 3.3 Segreti esposti
Chiavi nel codice, nei log, nei messaggi di errore mostrati all'utente, in un repository pubblico (→ A5). I bot che scandagliano GitHub in cerca di chiavi trovano e usano una chiave AWS **in pochi minuti**.

### 3.4 Dipendenze vulnerabili
Il tuo sistema è per il 90% codice di altri. Le vulnerabilità note vengono pubblicate, e da quel momento **chiunque può cercare chi non ha aggiornato**. Contromisura: uno strumento automatico che ti avvisa (`npm audit`, Dependabot) e una disciplina di aggiornamento. Non serve eroismo: serve costanza.

### 3.5 Caricamento di file
Un utente carica un file. Il file finisce in una cartella servita dal web. Il file è uno script. Ora l'utente esegue codice sul tuo server. **Regole:** i file caricati non stanno mai in una cartella eseguibile, si validano tipo e dimensione, si rinominano, e idealmente si servono da un dominio separato.

---

## 4. Il prompt di audit

Sessione nuova, contesto minimo (→ A8):

```
Fai un audit di sicurezza di questo codice. Sei un attaccante, non un
revisore cortese.

1. AUTORIZZAZIONE: per ogni endpoint e query, verifica che controlli non solo
   CHI è l'utente, ma che i dati richiesti gli appartengano. Elenca ogni punto
   in cui questo controllo MANCA. Prova a costruire una richiesta che accede
   ai dati di un altro utente/organizzazione.
2. INPUT: quali input non sono validati? Cosa succede con valori estremi,
   negativi, enormi, con caratteri speciali, con tipi inattesi?
3. SEGRETI: ci sono chiavi, token, password nel codice, nei log, nei messaggi
   di errore restituiti all'utente?
4. ERRORI: i messaggi di errore rivelano informazioni sull'interno del
   sistema (struttura del database, percorsi, versioni)?
5. DATI PERSONALI: quali dati personali vengono raccolti, dove finiscono
   (log inclusi), e chi può leggerli?
6. Per ogni problema: gravità, come sfruttarlo concretamente, come rimediare.
```

**Fallo girare a ogni rilascio che tocca dati o permessi.** Costa cinque minuti.

---

## 5. Il dato personale come passivo

Contabilmente, i dati personali **non sono un asset: sono un passivo**. Ogni dato raccolto è:
- un **obbligo** (proteggerlo, giustificarne la raccolta, cancellarlo su richiesta, saperlo esportare),
- un **rischio** (se esce, il danno è tuo: sanzione, notifica agli interessati, reputazione),
- un **costo** (sicurezza, backup, conformità).

Quindi la domanda su ogni campo che stai per aggiungere è: **"mi serve davvero, o lo raccolgo perché posso?"** Il codice fiscale di un relatore serve alla segreteria per il rimborso — non serve al sistema di gestione del programma scientifico. **Separali, e il secondo sistema non avrà mai un problema di privacy.**

Tre implicazioni pratiche che valgono per qualunque prodotto tocchi persone:
1. **Base giuridica** per ogni categoria di dati (perché ho il diritto di averlo).
2. **Cancellabilità** progettata nel modello (→ A7 §1.4): l'identità personale separata dai fatti che devono restare.
3. **Esportabilità**: l'interessato ha diritto ai suoi dati in formato leggibile. Se lo prevedi dall'inizio è un'ora; se lo aggiungi dopo è un progetto.

---

## Strumenti di questo modulo

1. **Il modello di minaccia in quattro domande**, con la tabella degli avversari (incluso te stesso).
2. **I sei principi** — in particolare: i controlli nell'interfaccia non sono sicurezza.
3. **Le cinque falle reali**, con l'autorizzazione mancante al primo posto.
4. **La difesa strutturale**: l'autorizzazione nello strato dati, non come controllo dimenticabile.
5. **Il test di isolamento fra clienti**, da scrivere una volta ed eseguire per sempre.
6. **Il prompt di audit avversariale.**
7. **Il dato personale come passivo**: la domanda "mi serve, o lo raccolgo perché posso?"

---

## Errori tipici

1. **Credere che i controlli nell'interfaccia proteggano qualcosa.**
2. **Verificare l'identità e non l'appartenenza dei dati** — la falla numero uno, e l'AI la introduce per omissione.
3. **Raccogliere dati "che magari serviranno".** Ogni campo è un passivo.
4. **Trattare i backup come meno sensibili dei dati vivi.** Contengono le stesse cose.
5. **Aggiornare mai le dipendenze**, perché "funziona".
6. **Non avere log.** Il giorno dell'incidente, l'assenza di log trasforma un problema in una crisi: non puoi dire cosa è successo, né a chi, né quando — e devi notificarlo comunque.

*v1.0 — riscrittura in profondità.*
