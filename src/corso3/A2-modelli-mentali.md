# A2 — I sei modelli mentali che spiegano ogni decisione tecnica

**Corso 3 · Parte A · v1.0**

Non ti serve il vocabolario: ti serve capire **perché il software si comporta come si comporta**. Sei modelli spiegano la quasi totalità delle decisioni che dovrai prendere o giudicare. Per ognuno: cos'è, perché genera bug, come lo usi per decidere, e la domanda diagnostica che rivela quando qualcuno (umano o AI) ti sta portando fuori strada.

---

## 1. Stato — dove vive la verità

**Il modello.** Tutto il software è gestione dello stato: cosa il sistema ricorda tra un'azione e la successiva. Un calcolo puro (dammi X, ti restituisco Y) è banale e non si rompe quasi mai. Lo stato è ciò che si rompe.

**Perché genera bug.** Perché lo stato può essere *duplicato* (la stessa informazione in due posti), *stantìo* (una copia che non sa di essere vecchia), o *conteso* (due azioni che lo modificano insieme). Praticamente ogni bug grave che vedrai in vita tua è una di queste tre cose.

**Il caso concreto.** Il numero di iscritti a un congresso compare in tre posti: la tabella iscrizioni, un contatore nella tabella evento (per non ricalcolarlo ogni volta), e la dashboard che lo mostra. Un'iscrizione fallisce a metà: la riga viene inserita, il contatore no. Da quel momento **hai due verità**, e non sai quale. Ogni report successivo è sospetto.

**Come si decide.** Per ogni dato, una sola domanda:

> **Qual è la fonte di verità, e chi ha il diritto di cambiarla?**

Se la risposta contiene un "beh, in realtà è anche in…", hai già un bug futuro. La verità sta in **un** posto; tutto il resto è *derivato* — e ciò che è derivato si ricalcola, non si memorizza (a meno che tu non stia deliberatamente ottimizzando, e allora lo scrivi nel Decision Log e ti prendi il carico di tenerlo coerente).

**Domanda diagnostica.** *"Se questa informazione esistesse in due posti e i due posti divergessero, come me ne accorgerei?"* Se la risposta è "non me ne accorgerei", non sei di fronte a un rischio: sei di fronte a una certezza a scoppio ritardato.

---

## 2. Confini — dove si taglia il sistema

**Il modello.** Un sistema è fatto di parti che comunicano attraverso confini (funzioni, moduli, API, servizi). La qualità di un'architettura **è** la qualità dei suoi confini: quelli buoni ti permettono di sostituire ciò che sta dentro senza toccare ciò che sta fuori.

**Il criterio che vale più di tutti.** Il confine giusto passa **dove le cose cambiano insieme**. Ciò che cambia per la stessa ragione, allo stesso momento, per volontà della stessa persona, sta dentro lo stesso confine. Ciò che cambia per ragioni diverse va separato. (È il *Single Responsibility Principle* nella sua formulazione seria: "una ragione per cambiare", non "fa una cosa sola".)

**Il caso concreto.** Il modulo che invia le email agli iscritti e quello che calcola i crediti ECM: sembrano vicini (riguardano lo stesso evento), ma cambiano per ragioni diversissime — le email cambiano quando cambia il marketing, i crediti quando cambia la normativa Agenas. Metterli insieme significa che ogni cambio di normativa rischia di rompere le email. **La vicinanza tematica non è un criterio; la co-variazione sì.**

**Come si decide.** Prima di approvare qualunque struttura proposta:

> **Se domani volessi buttare via questo pezzo e riscriverlo, quanto del resto dovrei toccare?**

Se la risposta è "molto", il confine è nel posto sbagliato — indipendentemente da quanto il codice sia elegante.

**Domanda diagnostica.** *"Elencami tutti i punti del sistema che dovrebbero cambiare se domani sostituissi [questo componente]."* Un modello onesto te li elenca, e la lunghezza della lista è la misura del tuo accoppiamento.

---

## 3. Accoppiamento e coesione — la fisica della manutenzione

**Il modello.** *Accoppiamento*: quanto una parte dipende dalle altre. *Coesione*: quanto ciò che sta insieme ha davvero a che fare. La regola ha cinquant'anni e non è invecchiata: **basso accoppiamento, alta coesione.**

**Perché ti riguarda anche se non leggi codice.** Perché l'accoppiamento è **il costo di ogni tua futura idea**. Un sistema molto accoppiato non è "brutto": è un sistema in cui *ogni tua richiesta costerà tre volte tanto*, e in cui l'AI, per accontentarti, romperà cose lontane.

**L'accoppiamento nascosto.** Quello visibile (A chiama B) è innocuo. Quelli che uccidono sono:
- **accoppiamento sui dati:** due moduli che leggono e scrivono la stessa tabella con assunzioni diverse su cosa significhi una colonna;
- **accoppiamento temporale:** B funziona solo se A è già stato eseguito, e nessuno lo ha scritto da nessuna parte;
- **accoppiamento semantico:** B *sa* come A rappresenta internamente le cose, e quindi A non può più cambiare idea.

**Domanda diagnostica.** *"Quali assunzioni implicite fa questo componente sul resto del sistema?"* — è il prompt che scopre più bug latenti di qualunque test.

---

## 4. Fallimento parziale — le cose si rompono a metà

**Il modello.** Nel software su rete, l'unità di fallimento non è "funziona / non funziona": è **"funziona a metà"**. La chiamata parte, il pagamento viene addebitato, la rete cade, la tua registrazione non avviene. Il denaro è uscito, il sistema non lo sa.

**I due concetti che ti servono.**

- **Idempotenza:** un'operazione è idempotente se eseguirla due volte ha lo stesso effetto di eseguirla una volta. "Imposta lo stato a *pagato*" è idempotente. "Aggiungi 100 € al saldo" **non lo è** — ed è il motivo per cui i doppi click generano doppi addebiti nei sistemi scritti male.
- **Transazione:** un insieme di operazioni che avvengono tutte o nessuna. Funziona benissimo **dentro un solo database**, e non funziona *attraverso* sistemi diversi (il tuo database e il fornitore dei pagamenti non possono stare nella stessa transazione: è un fatto fisico, non una limitazione tecnologica).

**Cosa fare quando la transazione non è possibile.** Si registra l'intenzione prima, si esegue, si riconcilia dopo. Il pattern (chiamato *outbox*, o più in generale *saga*) in linguaggio da committente è: **"scrivi che stai per farlo, fallo, scrivi che l'hai fatto — e prevedi un processo che ripulisca chi è rimasto a metà."**

**Le tre domande da fare su ogni operazione che tocca soldi, iscrizioni, o dati esterni:**
1. *Cosa succede se viene eseguita due volte?*
2. *Cosa succede se si interrompe a metà?*
3. *Come faccio ad accorgermene, e come rimedio?*

Se un progettista (umano o AI) non ha risposte a queste tre, il sistema **perderà dati**. Non "potrebbe": lo farà, e ti accorgerai al primo picco di traffico.

---

## 5. Costo del cambiamento — l'unica metrica di architettura

**Il modello.** Il software non ha costo di produzione: ha **costo di modifica**. Un'architettura non si giudica su "funziona" (funzionano anche le peggiori), ma su **quanto costa cambiare idea**.

**La conseguenza operativa.** Ogni scelta di oggi è un prezzo che paghi domani in flessibilità. Quindi la domanda architetturale non è "qual è la soluzione migliore?" ma:

> **Quali decisioni posso rimandare, e quali sono irreversibili?**

Le decisioni irreversibili (o costose da invertire) meritano tempo, discussione, ricerca. Quelle reversibili meritano una scelta rapida e un esperimento. Confonderle è il modo più comune di sprecare mesi.

**La scala di reversibilità** (usala per allocare il tuo tempo):

| Decisione | Costo di inversione | Quanto tempo dedicarci |
|---|---|---|
| Nome di una funzione, struttura di una pagina | Minuti | Zero: decidi e vai |
| Libreria di interfaccia, stile del codice | Giorni | Poco |
| Struttura delle API interne | Settimane | Un po' |
| **Schema del database, con dati veri dentro** | **Mesi** | **Molto** |
| **Modello di isolamento fra clienti** | **Riscrittura** | **Tutto quello che serve** |
| Fornitore cloud principale | Mesi–anni | Molto |

Questa tabella è il tuo bilancio: **il tempo che l'AI ti ha liberato va speso nelle righe in grassetto.**

---

## 6. Complessità essenziale e accidentale

**Il modello.** Fred Brooks (*No Silver Bullet*, 1986) distingue la complessità **essenziale** — quella del problema, che non puoi eliminare perché è il problema — da quella **accidentale**, introdotta dagli strumenti, dalle scelte, dalla storia.

**Perché conta oggi più che mai.** Gli strumenti AI riducono drasticamente la complessità accidentale: scrivere il codice, ricordare la sintassi, costruire l'impalcatura — tutta roba che costava e ora non costa. **Non toccano di un millimetro quella essenziale**: capire cosa serve, decidere i compromessi, gestire lo stato, definire i confini.

Da qui una previsione che vale come bussola: **il valore si sposta interamente sulla complessità essenziale.** Chi ha capito il dominio vince; chi sapeva solo scrivere codice vede il proprio vantaggio evaporare. Per te, che il dominio lo conosci meglio di qualunque sviluppatore, è la notizia migliore del decennio — a condizione che tu non sprechi il vantaggio cercando di diventare un mediocre programmatore.

---

## Strumento: il traduttore per committenti

Da usare quando qualcuno (o un modello) ti propone qualcosa e vuoi capire cosa stai davvero comprando.

| Ti dicono | Significa | Chiedi |
|---|---|---|
| "Lo spostiamo in un microservizio" | Aggiungiamo un confine di rete, con costi di latenza, fallimento e deploy | *Che problema risolve? Che complessità aggiunge? Possiamo ottenere lo stesso con un modulo ben separato?* |
| "Facciamo refactoring" | Cambiamo la struttura senza cambiare il comportamento | *Cosa migliora, concretamente? Come dimostriamo che il comportamento non è cambiato?* |
| "Deve scalare" | Deve reggere più carico | *Quanto carico, misurato dove? Abbiamo un numero o è immaginazione?* |
| "È temporaneo" | È permanente | *Cosa deve accadere perché venga rimosso, e chi lo farà? Scriviamolo nel Decision Log con una data.* |
| "Aggiungiamo una cache" | Creiamo una seconda copia della verità | *Cosa succede quando è stantìa? Chi la invalida? Abbiamo misurato la lentezza che stiamo curando?* |
| "Usiamo un ORM / un framework X" | Delego decisioni a un layer | *Cosa succede quando devo fare qualcosa che X non prevede?* |
| "Facciamo un'astrazione generica" | Prevedo il futuro | *Quanti casi d'uso reali abbiamo oggi? Se sono meno di tre, stiamo indovinando.* |

**La regola dietro l'ultima riga** (la più preziosa): *non astrarre prima di tre casi*. La generalizzazione fatta su un solo esempio è quasi sempre la generalizzazione sbagliata — e costa più della duplicazione che voleva evitare.

---

## Errori tipici

1. **Ottimizzare senza misurare.** Chi propone una cache, una denormalizzazione o un microservizio senza un numero, sta decorando.
2. **Confondere vicinanza tematica e co-variazione** nel disegnare i confini.
3. **Trattare tutte le decisioni come se fossero irreversibili** (paralisi) o **tutte come reversibili** (lo schema dati che ti perseguita per anni).
4. **Credere che l'AI riduca la complessità essenziale.** Non lo fa. La rende solo più visibile, perché toglie di mezzo il rumore.

*v1.0 — riscrittura in profondità.*
