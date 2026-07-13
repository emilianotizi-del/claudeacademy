# A3 — Specificare: il collo di bottiglia è diventato questo

**Corso 3 · Parte A · v1.0**

Con un esecutore che scrive in un minuto ciò che un team scriveva in una settimana, **la specifica non è più un documento burocratico: è il punto di leva**. Una specifica ambigua non produce un ritardo — produce un sistema sbagliato, costruito benissimo, in fretta.

---

## 1. Il paradosso della specifica

Una specifica completa, precisa e non ambigua **è** un programma. Se la scrivi al massimo dettaglio, hai programmato in prosa: hai solo cambiato linguaggio, e per il peggiore.

Quindi il mestiere non è "specificare tutto". È trovare il livello giusto:

> **Vincola le decisioni le cui conseguenze non puoi permetterti; lascia libere quelle il cui costo di errore è basso.**

Vincolare troppo poco → il modello decide per te cose che avrebbero dovuto essere tue (lo schema dati, il modello di permessi, il comportamento in caso di errore). Vincolare troppo → lentezza, e specifiche che nessuno tiene aggiornate, che è peggio di non averle.

**Il test per sapere se un dettaglio va nella specifica:** *se il modello lo decidesse a caso, e scoprissi la scelta tra due mesi, sarebbe un problema?* Se sì, va specificato. Se no, lascialo andare.

---

## 2. Anatomia di una specifica che funziona

Sette sezioni. Nessuna è opzionale, e quella che tutti saltano è la quinta.

### 2.1 Contesto e problema
Perché questa cosa esiste, chi soffre oggi. **Senza questa sezione, ogni compromesso verrà deciso a caso** — perché un compromesso è per definizione una scelta tra due mali, e la scelta dipende da cosa stai proteggendo.

### 2.2 Casi d'uso
Chi fa cosa, in che ordine. Scritti come narrazioni, non come elenchi di funzioni:

> *La segretaria riceve una mail da un relatore che vuole cambiare l'orario del suo intervento. Apre l'evento, trova la sessione, sposta l'intervento. Il sistema le mostra che così si sovrappone con un altro relatore. Lei decide comunque di procedere e lascia una nota. Il sistema avvisa via mail i due relatori coinvolti e il moderatore.*

Da questa narrazione si estraggono venti decisioni che nessun elenco di funzioni avrebbe fatto emergere: *si può forzare una sovrapposizione? chi viene avvisato? la nota è obbligatoria? il moderatore è un utente o solo un nome?*

**Le eccezioni sono il vero contenuto.** Il caso felice lo indovina chiunque, anche il modello. Il valore che porti tu sono i casi storti — e li conosci solo perché hai vissuto il dominio.

### 2.3 Modello dei dati
Le entità, le loro relazioni, e per ogni campo: obbligatorio o no, cosa lo rende valido, cosa succede quando cambia. È la parte che invecchia peggio se sbagliata (→ A7): merita metà del tempo totale della specifica.

### 2.4 Criteri di accettazione
"È fatto quando…", scritti in modo **osservabile e non opinabile**:

❌ *L'importazione degli iscritti deve funzionare bene.*
✅ *Data la lista `iscritti-2025.xlsx` (412 righe, 3 duplicati noti, 7 email malformate): il sistema importa 402 record, segnala i 3 duplicati con il criterio di rilevamento, rifiuta le 7 email malformate elencandole, e non modifica il file originale. L'operazione è ripetibile senza creare doppioni.*

Il secondo è verificabile da chiunque, anche fra sei mesi, anche da un estraneo. Il primo è un desiderio.

> **Se non sai dire quando è finito, non hai specificato: hai desiderato.**

### 2.5 Fuori scopo esplicito
**La sezione che tutti saltano e che decide se il progetto convergerà mai.** Non "cose che faremo dopo": **cose che non faremo, e perché**.

Il motivo è psicologico prima che tecnico: senza un fuori-scopo scritto, ogni conversazione con un utente aggiunge una funzione, e nessuna conversazione ne toglie. Il sistema cresce monotonicamente fino a diventare ingestibile. Il fuori-scopo è l'unica forza di richiamo.

### 2.6 Vincoli non funzionali
Sono **requisiti**, non aspirazioni, e vanno numerati:
- *Prestazioni*: la ricerca su 10.000 iscritti risponde sotto i 500 ms.
- *Sicurezza*: nessun utente vede dati di un'associazione diversa dalla propria (→ A9).
- *Privacy*: i dati personali cancellabili su richiesta entro 30 giorni.
- *Costi*: sotto i 50 €/mese fino a 20 associazioni.
- *Disponibilità*: se cade il sabato di un congresso, il danno è massimo → definire cosa succede.

### 2.7 Rischi e decisioni aperte
Ciò che ancora non sai. Scriverlo è un atto di igiene: **le decisioni aperte non dichiarate vengono prese implicitamente dall'esecutore**, e lo scoprirai quando saranno cementate.

---

## 3. Il template, pronto

```markdown
# Specifica · [COMPONENTE] · v0.1

## 1. Problema
[Chi soffre oggi, di cosa, e cosa fa al posto di questo sistema]

## 2. Utenti
| Chi | Cosa vuole ottenere | Con che frequenza | Livello tecnico |

## 3. Casi d'uso
### CU-1 · [nome]
**Attore:** ...
**Precondizioni:** ...
**Flusso principale:** 1) ... 2) ... 3) ...
**Eccezioni:** E1) se ... allora ... · E2) se ... allora ...
**Postcondizioni:** [cosa è vero nel sistema dopo]

## 4. Dati
[Entità, campi, vincoli di validità, cosa succede alla cancellazione,
 cosa deve essere storicizzato]

## 5. Criteri di accettazione
AC-1 · Dato [stato iniziale], quando [azione], allora [risultato osservabile]
AC-2 · ...

## 6. Fuori scopo
- NON faremo [X], perché [ragione]
- NON supporteremo [Y] in questa versione

## 7. Vincoli non funzionali
NF-1 · prestazioni: ...
NF-2 · sicurezza: ...
NF-3 · privacy: ...
NF-4 · costi: ...

## 8. Decisioni aperte
D-1 · [domanda] — da decidere entro [quando], impatta [cosa]
```

---

## 4. Tre tecniche che scovano ciò che non sai di non sapere

### 4.1 Il pre-mortem
Prima di approvare qualsiasi specifica:

```
Siamo tra sei mesi. Il progetto è fallito: abbiamo speso tempo e soldi e
il sistema non viene usato, oppure è stato abbandonato, oppure ha causato
un danno. Racconta i cinque scenari più plausibili di come ci siamo
arrivati, in ordine di probabilità. Per ciascuno: il primo segnale che
avremmo potuto notare, e quando.
```

Funziona per una ragione documentata in psicologia delle decisioni (Klein, 2007): **immaginare un fallimento già avvenuto sblocca informazioni che l'analisi prospettica dei rischi non produce**, perché rimuove la resistenza a criticare un piano che stiamo per adottare.

### 4.2 La revisione avversariale della specifica
Il prompt che uso sempre, prima di far scrivere una riga:

```
Ecco la specifica di [X]. NON implementarla.

1. Elenca ogni ambiguità che ti costringerebbe a indovinare. Per ciascuna,
   dimmi cosa faresti tu e perché — così vedo la decisione che starei
   delegando senza accorgermene.
2. Elenca i casi limite non coperti: dati mancanti o malformati, azioni
   concorrenti, fallimenti a metà, utenti malintenzionati, volumi 100 volte
   superiori al previsto.
3. Quali decisioni architetturali questa specifica mi sta facendo prendere
   implicitamente?
4. Quali requisiti sono in conflitto tra loro?
5. Scrivi i criteri di accettazione mancanti, in forma Dato/Quando/Allora.
```

Il punto 1 è il più prezioso e quasi nessuno lo chiede: **rende visibili le decisioni che stai delegando senza saperlo.**

### 4.3 L'inversione
Per ogni requisito, chiedi: *cosa succede se NON lo facciamo?* Se la risposta è "niente di grave", **non è un requisito** — è un desiderio che si è travestito. Questa singola domanda taglia tipicamente il 30% dello scopo di un progetto, e il 30% tagliato non manca mai a nessuno.

---

## 5. Specifiche eseguibili: il livello successivo

C'è una forma di specifica che vale doppio: quella **scritta come test**. Non devi saperli scrivere — devi saperli *leggere e commissionare*:

```
Da questi criteri di accettazione, scrivi i test che li verificano, prima
di scrivere l'implementazione. Mostrameli. Voglio leggere cosa affermano,
in italiano, prima di autorizzare il codice che li farà passare.
```

Perché è potente: **i test diventano la specifica che non può mentire**. Un documento può divergere dalla realtà silenziosamente per mesi. Un test che afferma il falso fallisce, rumorosamente, il giorno stesso. È l'unica forma di documentazione che si difende da sola (→ A8).

---

## Strumenti di questo modulo

1. **Template di specifica in 8 sezioni** — con fuori-scopo obbligatorio.
2. **Criteri di accettazione Dato/Quando/Allora** — osservabili, non opinabili.
3. **Prompt di pre-mortem** — per far emergere i fallimenti prima che accadano.
4. **Prompt di revisione avversariale** — con la domanda sulle decisioni delegate implicitamente.
5. **L'inversione** — per tagliare il 30% di scopo che nessuno rimpiangerà.

---

## Errori tipici

1. **Specificare le funzioni invece dei casi d'uso.** "Deve avere un pulsante esporta" non dice a chi serve, quando, e cosa deve contenere il file.
2. **Saltare il fuori-scopo**, e poi chiedersi perché il progetto non finisce mai.
3. **Criteri di accettazione soggettivi** ("deve essere veloce", "intuitivo").
4. **Trattare la specifica come un documento morto.** Se il sistema cambia e la specifica no, la specifica sta mentendo — e prima o poi qualcuno le crederà.
5. **Specificare da soli.** La specifica migliora enormemente se la fai criticare da un modello *prima* di implementarla: costa cinque minuti e ti risparmia riscritture.

*v1.0 — riscrittura in profondità.*
