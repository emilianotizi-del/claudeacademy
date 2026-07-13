# A7 — I dati: la decisione che si paga per anni

**Corso 3 · Parte C · v1.0**

> **Il codice si riscrive in un pomeriggio. I dati no.**

Questa asimmetria è il fatto centrale della tua nuova economia. L'AI ha reso il codice quasi gratuito e **non ha toccato di un millimetro** il costo di sbagliare il modello dei dati: perché una volta che ci sono dentro dati veri, di clienti veri, cambiarlo significa migrazioni, incoerenze, tempi di fermo, e la possibilità concreta di perdere informazione irrecuperabile.

**Il tempo che l'AI ti ha liberato va speso qui.** È il miglior investimento dell'intero corso.

---

## 1. Le sette domande che rivelano un modello sbagliato

Da fare su ogni entità, prima di scrivere una riga.

### 1.1 Qual è la fonte di verità?
Se un'informazione esiste in due posti, **divergeranno**. Non "potrebbero": lo faranno, e lo scoprirai da un cliente. Ciò che può essere *derivato* (il numero di iscritti = conta le iscrizioni) non va memorizzato — a meno che tu non stia deliberatamente ottimizzando, e allora è una decisione da scrivere nel Decision Log, con l'onere esplicito di mantenerla coerente.

### 1.2 Cosa cambia nel tempo, e la storia serve?
**Questa è la domanda che distingue i sistemi seri dai giocattoli.**

Se memorizzi solo lo stato corrente, **cancelli il passato a ogni aggiornamento**. Sembra innocuo, finché non arriva la domanda: *"chi erano i soci in regola il giorno dell'assemblea?"*, oppure *"quanto abbiamo fatturato a quello sponsor l'anno scorso, prima che cambiasse ragione sociale?"*

In un **sistema di registro** — e ogni sistema che gestisce iscrizioni, quote, cariche, crediti formativi lo è — **la storia è il prodotto**, non un extra. Il che porta alla distinzione più importante di questo modulo:

| Approccio | Cosa memorizzi | Quando usarlo |
|---|---|---|
| **Stato corrente** | "Mario è socio attivo" | Dati che non hanno rilevanza storica: preferenze, impostazioni |
| **Eventi + stato** | "14/03/2024: Mario si è iscritto · 12/01/2026: quota 2026 pagata" — e lo stato è *derivato* | Tutto ciò che ha rilevanza contabile, legale, formativa, o su cui qualcuno potrebbe contestare |

**La regola:** se qualcuno potrebbe un giorno chiederti *"com'era in quel momento?"*, oppure *"perché è cambiato?"*, **devi registrare eventi**. Aggiungere la storia dopo è quasi sempre impossibile: l'informazione non è stata persa — non è mai stata scritta.

### 1.3 Le identità sono stabili?
Un identificativo **non deve mai cambiare** e **non deve avere significato**. L'errore classico è usare l'email come chiave del socio: poi qualcuno la cambia, e tutti i riferimenti storici si sfaldano. Ogni entità ha un identificativo tecnico, immutabile, senza senso per l'uomo. Il codice fiscale, la mail, la matricola sono *attributi*, non identità.

### 1.4 Cosa succede alle cancellazioni?
Tre scelte, tre conseguenze diverse:
- **Cancellazione reale:** il dato sparisce. Semplice, ma perdi lo storico e rompi i riferimenti.
- **Soft delete** (un flag `cancellato`): mantieni tutto, ma **ogni singola query del sistema deve ricordarsi di filtrarlo** — e prima o poi una se ne dimenticherà, mostrando a un utente dati che credeva cancellati. È un vantaggio che si paga con una tassa perpetua.
- **Archiviazione:** il dato si sposta in una tabella dedicata. Le query normali restano pulite, lo storico resta accessibile. **È quasi sempre la scelta giusta** — e va decisa all'inizio.

Sopra tutto questo c'è il **diritto alla cancellazione**: se un socio esercita il diritto all'oblio, il dato personale deve sparire davvero. Il che significa che il tuo modello deve poter **separare** l'identità personale (cancellabile) dai fatti storici che devono restare (una quota incassata, un credito ECM erogato). Questo si progetta prima: si tiene l'anagrafica in un posto e i fatti in un altro, legati da un identificativo tecnico. Dopo, è una riscrittura.

### 1.5 Cosa rende valido un dato?
Ogni campo ha regole (una quota ha un anno, un socio ha uno stato tra N possibili, una data di fine è dopo quella di inizio). **Dove vivono queste regole?** Se vivono solo nel codice dell'interfaccia, verranno violate — da un'importazione, da uno script, da un'altra parte del sistema, da un agente che ha scritto una query di fretta.

**Le regole che non possono essere violate vivono nel database**, sotto forma di vincoli. È l'unico posto che nessuno può aggirare per distrazione:
- un iscritto deve puntare a un evento che esiste (chiave esterna);
- non possono esistere due iscrizioni della stessa persona allo stesso evento (vincolo di unicità);
- una quota non può essere negativa (vincolo di controllo).

**Ogni vincolo che metti nel database è una classe di bug che non potrà mai verificarsi.** Nessun test è altrettanto potente, perché il test verifica ciò a cui hai pensato; il vincolo blocca anche ciò a cui non hai pensato.

### 1.6 Come si evolve?
Il modello cambierà. La domanda è: **come, senza fermare tutto?** La risposta si chiama *migrazione*: un file versionato, applicato in ordine, che trasforma lo schema. Con dati veri dentro, la migrazione **deve essere reversibile** e va provata su una copia. (Il pattern serio per i cambi rischiosi: aggiungi il nuovo campo → scrivi in entrambi → migra i vecchi dati → leggi dal nuovo → rimuovi il vecchio. Quattro rilasci invece di uno, e nessun downtime.)

### 1.7 Chi può vedere cosa?
Nei prodotti multi-cliente questa **non** è una domanda sull'interfaccia: è una domanda sui dati (→ A9, e A17 per il multi-tenancy). Se la separazione tra clienti vive solo nel codice applicativo, **una singola query dimenticata espone i dati di un cliente a un altro**. Vive nel database, o non vive.

---

## 2. Normalizzare, denormalizzare, e chi decide

**Normalizzare** = ogni fatto in un posto solo. È il punto di partenza corretto, sempre.

**Denormalizzare** = duplicare deliberatamente per andare più veloce. È un'ottimizzazione che **si paga in coerenza**: da quel momento hai due copie e il dovere eterno di tenerle allineate.

**La regola di ingaggio:** chi propone di denormalizzare **deve portare una misura**, non un timore. "Potrebbe essere lento" non è un argomento. "La query impiega 4 secondi su 50.000 righe, ecco il profilo di esecuzione" lo è.

> **Ottimizzare prima di misurare è il modo più diffuso di rendere un sistema fragile senza renderlo veloce.**

Vale anche per gli indici, per le cache, per tutto: prima misuri, poi intervieni, poi misuri di nuovo per verificare che sia servito. Un modello, se glielo chiedi, ti aggiungerà volentieri cinque indici "per sicurezza": ogni indice rallenta le scritture e occupa spazio. Non è gratis.

---

## 3. Esempio completo: quote e rinnovi

Prendiamo il caso più semplice possibile e vediamo come si sbaglia.

### Il modello ingenuo
```
soci: id, nome, email, stato ("attivo"/"non in regola"), quota_pagata (sì/no)
```
Funziona il primo anno. Poi:
- *Chi era in regola all'assemblea di marzo?* — **informazione perduta**: `quota_pagata` è stata sovrascritta.
- *Un socio paga la quota 2026 a dicembre 2025.* — dove lo metto?
- *Ha pagato due anni fa ma non l'anno scorso.* — il modello non lo sa rappresentare.
- *Ha cambiato email.* — se qualcosa la usava come riferimento, si rompe.

### Il modello che regge
```
soci:      id (tecnico, immutabile), nome, email, data_iscrizione
quote:     id, socio_id, anno, importo, stato, data_pagamento, riferimento_pagamento
eventi_socio: id, socio_id, tipo (iscritto/sospeso/dimesso/riattivato), data, motivo
```
Ora:
- Lo **stato corrente** ("è in regola?") è **derivato**: esiste una quota pagata per l'anno in corso?
- Lo **stato al 15 marzo 2024** è ricostruibile: guardo le quote e gli eventi con data ≤ 15/03/2024. **La storia c'è.**
- Il pagamento anticipato è banale (la quota ha un `anno` proprio, distinto dalla data di incasso).
- L'email cambia senza rompere nulla, perché l'identità è il campo `id`.
- Il diritto all'oblio è gestibile: anonimizzo `soci`, i fatti contabili in `quote` restano.

**La differenza tra i due modelli è mezz'ora di pensiero.** La differenza nei costi, a due anni, è di settimane di lavoro e di un contenzioso potenziale.

---

## 4. Il prompt di revisione del modello dati

Da usare **sempre**, prima di approvare uno schema — e in una sessione pulita, non in quella che l'ha prodotto:

```
Ecco il modello dati proposto per [SISTEMA]: [schema].
Non implementarlo. Analizzalo come farebbe un DBA ostile:

1. Per ogni dato: qual è la fonte di verità? Ci sono duplicazioni implicite?
2. Quali informazioni vengono PERSE per sempre a ogni aggiornamento? Sono
   informazioni che qualcuno potrebbe voler ricostruire in futuro (per un
   audit, un contenzioso, un adempimento normativo)?
3. Quali identificativi hanno significato per l'uomo e quindi potrebbero
   cambiare? Cosa si rompe se cambiano?
4. Quali regole di integrità dovrebbero essere vincoli nel database e non
   sono? Elencale come istruzioni SQL.
5. Cosa succede se un utente esercita il diritto alla cancellazione? Quali
   dati devono restare per obbligo di legge, e il modello lo permette senza
   contraddizioni?
6. Se questo sistema servisse più clienti, dove passerebbe la separazione?
   È applicabile a livello di database, o solo nel codice?
7. Quali sono le tre query più frequenti che questo modello rende costose?
```

Le domande 2 e 5 sono quelle che salvano i progetti. La 6 è quella che, se ignorata, impone una riscrittura quando arriva il secondo cliente.

---

## Strumenti di questo modulo

1. **Le sette domande** sul modello dati.
2. **La distinzione stato vs eventi** — e la regola: se qualcuno può chiedere "com'era allora?", registri eventi.
3. **La regola dell'identità**: tecnica, immutabile, senza significato.
4. **I vincoli nel database** come classi di bug rese impossibili.
5. **La regola di ingaggio sull'ottimizzazione**: nessuna denormalizzazione senza misura.
6. **Il prompt di revisione avversariale del modello.**

---

## Errori tipici

1. **Sovrascrivere lo stato** e scoprire tre anni dopo che serviva la storia. È il danno irreversibile per eccellenza: i dati non sono stati persi, non sono mai stati scritti.
2. **Usare un dato con significato come identità** (email, codice fiscale).
3. **Mettere le regole solo nell'interfaccia**, dove chiunque le aggira.
4. **Soft delete ovunque**, e la tassa perpetua del filtro dimenticato.
5. **Denormalizzare per paura**, senza misura.
6. **Rimandare la domanda del multi-cliente**, e pagarla con una riscrittura.

*v1.0 — riscrittura in profondità.*
