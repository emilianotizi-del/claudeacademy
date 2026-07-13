# A17 — Il metodo: dal problema al sistema che regge

**Corso 3 · Parte E · v1.0** — *sintesi operativa*

---

## 1. Il ciclo, in nove passi

Ogni sistema che costruirai, dal più piccolo al più ambizioso, passa da qui. I passi non si saltano — si *dimensionano*: per uno strumento interno da due giorni, il passo 3 sono venti minuti; per un prodotto, due settimane.

| # | Passo | Deliverable | Modulo |
|---|---|---|---|
| 1 | **Problema** | Chi soffre, oggi, di cosa. Osservato, non intervistato. | A14 |
| 2 | **Decisione di esistere** | Vale la pena per 5 anni, con tutti i costi? Cosa non farò intanto? | A16 |
| 3 | **Carta** | Cosa è, cosa non è, criteri di morte | A1 |
| 4 | **Specifica** | Casi d'uso, dati, criteri di accettazione, fuori scopo | A3 |
| 5 | **Dati** | Il modello — con la storia, le identità, i vincoli | A7 |
| 6 | **Fetta verticale** | Un pezzo completo che funziona davvero, non il tutto abbozzato | — |
| 7 | **Verifica** | Progettata prima: test, invarianti, revisione avversariale | A8, A9 |
| 8 | **Produzione** | Piccola, reversibile, osservata | A12 |
| 9 | **Governo** | Manutenzione, debito, costi, successione | A13, A16 |

**Il passo 6 merita una spiegazione**, perché è quello che quasi tutti sbagliano.

---

## 2. La fetta verticale

Il modo in cui i sistemi seri nascono **non è** "costruiamo tutti i pezzi e poi li colleghiamo". È: **un pezzo completo, sottile, che attraversa tutti gli strati e funziona davvero.**

Non: *facciamo tutto il database, poi tutte le API, poi tutta l'interfaccia.* Quel percorso ti dà tre cose incomplete e nessun feedback, per settimane.

Ma: *facciamo "aggiungere un socio e vederlo nell'elenco" — dall'interfaccia al database, con i test, con i permessi, in produzione.* Poi il pezzo successivo.

**Perché è superiore, per tre ragioni indipendenti:**
1. **Scopri i problemi veri subito.** L'integrazione tra strati è dove vivono le sorprese, e la scopri il primo giorno invece che alla fine.
2. **Hai qualcosa da mostrare**, quindi hai feedback vero (→ A14 §2.5), non opinioni su un documento.
3. **Se ti fermi, hai qualcosa che funziona.** I progetti si fermano — per cambio di priorità, per stanchezza, per un'emergenza. Chi ha costruito a strati orizzontali, quando si ferma, ha zero.

**La scelta della prima fetta:** quella che dimostra il valore centrale, tocca tutti gli strati, e che qualcuno userebbe *da subito* anche se fosse l'unica cosa che esiste.

---

## 3. Il multi-tenancy, se il tuo sistema servirà più clienti

Questa è **l'unica decisione veramente irreversibile** di tutto l'elenco, e va presa al passo 5, non quando arriva il secondo cliente.

**Il problema:** i dati del cliente A non devono mai, in nessuna circostanza, per nessun errore, essere visibili al cliente B. Non è un requisito di sicurezza tra gli altri: **è il requisito che, se violato una sola volta, chiude un'azienda.**

**Le tre strategie:**

| Strategia | Come | Pro | Contro |
|---|---|---|---|
| **Database separati** | Un database per cliente | Isolamento fisico: un errore nel codice non può causare una fuga | Costoso, migrazioni moltiplicate, complesso oltre le poche decine di clienti |
| **Schema separati** | Un database, uno schema per cliente | Buon isolamento, costo medio | Complessità di gestione |
| **Riga con `tenant_id`** | Una tabella, una colonna che dice di chi è la riga | Semplice, economico, scala | **Ogni singola query deve filtrare. Una query che dimentica = fuga di dati.** |

**La terza è la scelta più comune** ed è ragionevole — **a una condizione non negoziabile:** il filtro **non deve dipendere dal fatto che qualcuno si ricordi di scriverlo.**

Va imposto nello strato dei dati: in Postgres, le *row-level security policies* fanno esattamente questo — il database rifiuta di restituire righe di altri tenant, **anche se la query dimentica il filtro**. È la differenza tra un sistema che perdona gli errori e uno che li trasforma in incidenti da prima pagina.

> **Con un esecutore-AI che genera query, questo non è un consiglio: è una necessità strutturale.** Prima o poi genererà una query senza il filtro. Deve trovare un muro, non una porta aperta.

**Il test da scrivere il primo giorno, e da eseguire per sempre:** *due tenant, login come utente del primo, tentativo di accesso a ogni risorsa del secondo per identificativo diretto. Deve fallire ovunque.*

---

## 4. Le otto domande prima di costruire qualsiasi cosa

Da rileggere ogni volta che stai per iniziare. **Costano dieci minuti e ti hanno già risparmiato mesi.**

1. **Chi ha questo problema**, e me lo pagherebbe — in denaro o in tempo risparmiato?
2. **Cosa fa oggi** al posto mio, e perché dovrebbe cambiare abitudine?
3. **Esiste già?** E se sì: perché non lo compro?
4. **Qual è la fetta più piccola** che serve già a qualcuno?
5. **Cosa succede a chi lo usa se lo abbandono?**
6. **Quanto costa tenerlo vivo per cinque anni** — in tempo mio, non in euro?
7. **Cosa non starò facendo**, mentre faccio questo?
8. **Se avessi una sola settimana, cosa costruirei?** — *e la risposta a questa domanda è quasi sempre la cosa giusta da costruire per prima.*

---

## 5. Gli artefatti permanenti: il tuo apparato

Riassunto di tutto ciò che, se esiste, ti rende un architetto, e se non esiste ti rende un utente fortunato.

| Artefatto | Dove | Cosa impedisce |
|---|---|---|
| **Carta di progetto** | `docs/carta.md` | Di costruire la cosa sbagliata, e di non fermarti mai |
| **Specifica** | `docs/spec-*.md` | Di delegare decisioni senza accorgertene |
| **Decision Log** | `docs/decisions.md` | Che il sistema contraddica se stesso, e che la teoria muoia |
| **Istruzioni per l'agente** | `CLAUDE.md` | Che ogni sessione riparta da presupposti diversi |
| **Comando unico di verifica** | `npm run check` | Che tu debba fare il compilatore a mano |
| **Test delle regole di dominio** | accanto al codice | Che una modifica rompa in silenzio ciò che conta |
| **Test di isolamento fra clienti** | nella suite | La fine dell'azienda |
| **Set di valutazione AI** | `evals/` | Che le modifiche ai prompt siano scommesse cieche |
| **Osservabilità** | in produzione | Di scoprire i guasti dai clienti |
| **Backup provato** | fuori dal sistema | La perdita irreversibile |
| **Manutenzione mensile** | **in calendario** | L'invecchiamento silenzioso |

**Nessuno di questi richiede di saper programmare.** Tutti richiedono di **decidere che esistano**, e di rifiutarsi di procedere quando non ci sono. È esattamente il lavoro del committente tecnico.

---

## 6. La disciplina, in una pagina

L'AI ha spostato il vincolo. Non produrre: **decidere bene, e verificare**.

La tentazione, con un esecutore instancabile, è **costruire di più**. La disciplina è **costruire meno, e capire ciò che si costruisce**.

> **Un sistema che comprendi è un sistema che puoi cambiare.**
> **Un sistema che non comprendi ti possiede** — anche se l'hai commissionato tu, anche se è tutto tuo, anche se funziona.

E il possesso si manifesta sempre nello stesso modo: nel giorno in cui devi cambiarlo in fretta, e non puoi.

---

## 7. La chiusura

La domanda non è più **«so costruirlo?»**. Quella è diventata banale, e la banalità è precisamente il motivo per cui non vale più niente.

La domanda è:

> **«So decidere se debba esistere? So specificarlo? So accorgermi quando è sbagliato? So mantenerlo vivo? E so quando fermarmi?»**

Il resto è esecuzione. E l'esecuzione, ormai, è abbondante.

---

## L'intero corso, in dodici righe

1. Il codice è gratis. La comprensione no. Ogni riga che non capisci è debito. **(A1)**
2. Le decisioni irreversibili meritano tempo; le altre, no. **(A2)**
3. Se non sai dire quando è finito, non hai specificato: hai desiderato. **(A3)**
4. Piano prima dell'azione; sessioni corte; consolida sempre. **(A4)**
5. La velocità sostenibile è una funzione della reversibilità. **(A5)**
6. Costruisci ciò che ti differenzia, compra il resto, evita il superfluo. **(A6)**
7. Il modello dati è la decisione che si paga per anni: se qualcuno può chiedere "com'era allora?", registri eventi. **(A7)**
8. Non verificare tutto: rendi impossibile che l'importante passi inosservato. **(A8)**
9. L'AI non aggiunge vulnerabilità: dimentica controlli. Metti l'autorizzazione dove non si può dimenticare. **(A9)**
10. Non chiedere "risolvi": chiedi "perché accade". **(A10, A11)**
11. Rilascia piccolo, spesso, reversibile — e conta le righe prima e dopo ogni migrazione. **(A12)**
12. Il codice è commodity; il dominio no. **(A14, A15, A16)**

*v1.0 — sintesi; sostituisce il precedente A15.*
