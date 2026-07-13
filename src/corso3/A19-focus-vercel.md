# A19 — Focus: Vercel, dal punto di vista dell'architetto

**Corso 3 · Parte F · Focus di stack · v1.0**
*Fonti verificate il 13/07/2026: vercel.com/docs (deployment protection, environment variables, functions).*

Vercel è **una macchina che trasforma un commit in un sito online**, con una rete di distribuzione davanti e delle funzioni che girano su richiesta. Il valore è reale: elimina un'intera categoria di lavoro (server, deploy, certificati, scalabilità). **Il prezzo è che alcune decisioni importanti vengono prese per te, e altre — le più pericolose — restano tue senza che tu te ne accorga.**

---

## 1. Il modello mentale

```
git push  ──►  build  ──►  deploy immutabile  ──►  URL
                                 │
      ┌──────────────────────────┼──────────────────────────┐
   PRODUZIONE                 PREVIEW                    LOCALE
   (branch main)         (ogni altro branch/PR)         (vercel dev)
```

**Tre proprietà da capire subito:**

1. **Ogni deploy è immutabile e ha il suo URL.** Non "aggiorni" il sito: ne pubblichi uno nuovo e l'indirizzo di produzione punta a quello. **Conseguenza magnifica: il ritorno indietro è istantaneo** (si ripunta al deploy precedente). È la reversibilità di A5, regalata.

2. **Ogni branch ottiene un ambiente di preview vero e proprio**, con il suo URL. È lo strumento di feedback più potente che hai (→ A14 §2.5): mandi un link, la persona guarda, ti dice cosa non va. Ma vedi §3: **quel link, per default, potrebbe essere aperto a chiunque.**

3. **Il codice gira in due posti diversi**, e confonderli è la fonte di metà degli errori: **sul server** (le funzioni, che possono usare i segreti) e **nel browser** (il codice che chiunque può leggere).

---

## 2. La distinzione che devi tatuarti: server o browser?

**È il confine più importante di tutto lo stack**, e l'errore che genera è sempre lo stesso e sempre grave.

| | Gira sul **server** | Gira nel **browser** |
|---|---|---|
| Chi lo vede | Nessuno | **Tutti.** È testo, si legge con due clic |
| Può usare segreti? | **Sì** | **Mai** |
| Variabile d'ambiente | `DATABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY` | Solo `NEXT_PUBLIC_*` |

> **[FATTO] Il prefisso `NEXT_PUBLIC_` (o `PUBLIC_`, secondo il framework) significa letteralmente: "metti questo valore nel bundle che scarica ogni visitatore".**

Da qui **l'errore più comune e più costoso dell'intero stack Vercel+Supabase**: qualcosa non funziona nel browser, la variabile risulta `undefined`, l'agente (o tu, di fretta) aggiunge il prefisso `NEXT_PUBLIC_` per farla funzionare — **e se quella variabile era la chiave `service_role`, hai appena pubblicato l'accesso root al database a ogni visitatore.** Il sito funziona benissimo. È il momento peggiore possibile per un successo apparente.

**La regola per il file di istruzioni del progetto** (→ A4):
```
MAI aggiungere il prefisso NEXT_PUBLIC_ a una variabile per "farla
funzionare". Se una variabile non è disponibile nel browser, il codice che
la usa sta girando nel posto sbagliato: spostalo sul server.
Le sole variabili NEXT_PUBLIC_ ammesse in questo progetto sono: [elenco].
Aggiungerne una richiede la mia approvazione esplicita.
```

**Il controllo che devi fare a ogni rilascio:** cerca la chiave segreta nel bundle pubblicato. Se la trovi, revoca e rigenera **prima** di qualunque altra cosa (→ A5 §4).

---

## 3. Preview: la comodità che diventa una fuga

[FATTO, docs] Le preview sono URL reali, raggiungibili. Vercel offre la **Deployment Protection** (protezione con autenticazione Vercel o con password, secondo il piano) — **ma va attivata e verificata.**

Le due insidie:

**a) Preview aperte con dati veri.** Il caso classico: la preview punta al database di produzione "per comodità di test". Ora esiste un URL non indicizzato ma pubblico che espone i dati veri, e sopravvive per sempre nella cronologia dei deploy. **La regola:** le preview usano un database di prova, o sono protette. Mai la coppia "aperta + dati veri".

**b) Segreti diversi per ambiente.** [FATTO, docs] Le variabili si impostano per ambiente (production / preview / development) e si possono legare a un branch specifico. **Usa questa separazione**: la preview non deve poter toccare la produzione (→ A5 §2). Se il token di preview ha i permessi di produzione, la separazione degli ambienti è teatro.

**c) L'accesso automatico** (test, agenti, CI) su deploy protetti si fa con il *bypass secret* (`VERCEL_AUTOMATION_BYPASS_SECRET`), non abbassando la protezione. Proteggere e poi disattivare la protezione "perché i test non passano" è il modo in cui le buone intenzioni diventano incidenti.

---

## 4. Le funzioni: dove gira il tuo codice, e cosa costa

Il modello serverless ha proprietà che cambiano l'architettura, e vanno conosciute prima, non scoperte in bolletta.

**1 · Le funzioni sono senza stato e senza memoria.** Non puoi tenere qualcosa "in memoria tra una richiesta e l'altra": ogni invocazione può girare su una macchina diversa. Lo stato **vive nel database, sempre**. È un vincolo salutare (→ A2), ma va compreso: la cache in una variabile globale, che nell'ambiente locale funziona benissimo, in produzione produce risultati incoerenti — e questo è **esattamente** il tipo di bug "funziona in locale, non in produzione" di A10.

**2 · Le funzioni hanno un tempo massimo.** Un'operazione lunga (importare 5.000 soci, generare 400 PDF, inviare 400 email) **verrà interrotta a metà**. La conseguenza è pesante e non teorica: se l'operazione non è idempotente (→ A2), l'utente riprova e **duplica**.
**Le operazioni lunghe vanno spezzate o messe in coda** — e sono idempotenti. Questa è una decisione di architettura, non un dettaglio di implementazione, e va presa nella specifica.

**3 · Il cold start.** Una funzione che non gira da un po' impiega di più alla prima chiamata. Su un uso a picchi — un congresso, un'apertura iscrizioni — significa che i **primi** utenti hanno l'esperienza peggiore, ed è vero il contrario di ciò che il buon senso suggerisce.

**4 · Il costo cresce con l'uso, e con la durata.** È il modello a consumo di A15 §5, applicato al calcolo. **La domanda da fare prima:** *cosa succede alla bolletta se una funzione va in loop, o se qualcuno la chiama diecimila volte?* Esistono limiti di spesa: **impostali il primo giorno**, non dopo la prima brutta sorpresa. Ed è il tipo di sorpresa che arriva in un fine settimana.

---

## 5. Il vantaggio strategico da sfruttare davvero

Vercel ti regala tre cose che, in A5 e A12, ho descritto come faticose da costruire:

| Proprietà | Cosa ti dà |
|---|---|
| **Deploy atomico e immutabile** | Il ritorno indietro istantaneo. **Usalo:** al primo segnale di guaio in produzione, si torna indietro *e poi* si indaga (→ A12 §5) |
| **Preview per ogni branch** | Il feedback vero, su qualcosa che si tocca (→ A14). È la leva più sottovalutata: **mostra, non descrivere** |
| **Rilasci piccoli e frequenti, gratis** | Il risultato DORA (→ A12 §3): il pattern più sicuro è anche quello che la piattaforma rende più facile |

**Il consiglio operativo:** il fatto che rilasciare costi zero **non è un invito a rilasciare senza controlli** — è un invito a rilasciare **pezzi piccoli**, ognuno verificato. La differenza tra usare bene e usare male questa piattaforma sta tutta qui: chi non ha disciplina, con Vercel, sbaglia semplicemente più in fretta.

---

## 6. Il lock-in, valutato onestamente

**Quanto sei legato?** Dipende quasi interamente da te:
- Se usi solo build e hosting: **te ne vai in un giorno.** Il lock-in è basso, e Vercel è un'ottima scelta.
- Se usi le loro funzioni, il loro storage, la loro cache, il loro middleware: **te ne vai in un mese.**
- Se la logica di dominio è intrecciata alle loro API: **non te ne vai** (→ A6 §4).

**La contromisura è quella nota:** la logica di dominio non deve sapere dove gira. Un modulo che parla con la piattaforma; il resto del sistema, no. Costa poco all'inizio e vale moltissimo il giorno in cui i prezzi cambiano — o il giorno in cui un cliente istituzionale ti chiede dove vivono fisicamente i suoi dati, e la risposta deve essere una tua scelta, non una conseguenza.

---

## Checklist Vercel

```
□ Nessun segreto in una variabile NEXT_PUBLIC_* — verificato cercandolo
  nel bundle pubblicato
□ Variabili separate per production / preview / development
□ Le preview NON puntano al database di produzione (o sono protette)
□ Deployment Protection attiva sulle preview, se i dati non sono finti
□ Limiti di spesa impostati
□ Nessuna operazione lunga dentro una funzione: spezzata o in coda
□ Ogni operazione ripetibile è idempotente
□ Il ritorno indietro è stato provato almeno una volta, non solo letto
□ La logica di dominio non dipende dalle API della piattaforma
```

---

## Errori tipici

1. **`NEXT_PUBLIC_` aggiunto "per far funzionare la cosa".** È il modo in cui i segreti finiscono online, ed è quasi sempre un agente che lo propone e un umano di fretta che accetta.
2. **Preview aperte sui dati di produzione.**
3. **Stessi segreti in tutti gli ambienti**, che vanifica la separazione.
4. **Operazioni lunghe dentro una funzione**, con timeout e duplicati.
5. **Nessun limite di spesa.** Il modello a consumo non perdona i loop.
6. **Confondere "il deploy è facile" con "il rilascio è sicuro".** La piattaforma ti dà la reversibilità; la disciplina la devi mettere tu.

*v1.0 — focus di stack; fatti verificati su vercel.com/docs il 13/07/2026.*
