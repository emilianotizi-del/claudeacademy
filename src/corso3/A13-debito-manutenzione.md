# A13 — Debito tecnico e manutenzione: la fisica dell'invecchiamento

**Corso 3 · Parte D · v1.0**

---

## 1. La metafora, nella sua versione corretta

Ward Cunningham, che coniò il termine, **non** intendeva "codice scritto male". Intendeva questo:

> **Il debito tecnico è la distanza tra come hai costruito il sistema e come lo capisci ora.**

Si contrae **consapevolmente** per andare più veloce: *spediamo così, sappiamo che non è la struttura giusta, la sistemeremo quando avremo capito meglio.* È una scelta legittima, spesso ottima. Diventa distruttiva quando **non la si ripaga** — e quando **non si sa di averla contratta**.

Il codice scritto male da chi non sapeva fare meglio non è debito: è **incompetenza**, e non ha gli stessi rimedi.

---

## 2. Le quattro specie, con rimedi diversi

| Specie | Origine | Rimedio |
|---|---|---|
| **Deliberato** | "Facciamo così per ora, e lo scriviamo" | Ripagare quando serve. **Ha una data e un motivo nel Decision Log.** |
| **Da ignoranza** | Allora non sapevi | Si scopre imparando. Si ripaga quando emerge, senza colpevolizzarsi |
| **Da entropia** | Le dipendenze invecchiano, le API dei fornitori cambiano, il mondo si muove | **Arriva anche se non tocchi niente.** Manutenzione periodica obbligatoria |
| **Da comprensione perduta** | Il codice c'è, funziona, **nessuno sa più perché è così** | Il più costoso. Prevenzione: Decision Log. Cura: archeologia |

**La quarta è la specie nuova dell'era degli agenti**, e merita la sezione seguente.

---

## 3. Il debito che l'AI genera (ed è un debito diverso)

Un esecutore-AI abbassa drasticamente il costo di **produrre** codice e **non abbassa di nulla** il costo di **comprenderlo**. Questa asimmetria genera una forma di debito che prima non esisteva su questa scala:

> **Sistemi grandi, ordinati, plausibili, che nessuno ha mai avuto in testa.**

Il codice generato è tipicamente *più pulito della media*: nomi sensati, struttura ordinata, commenti. Manca una cosa sola: **qualcuno che sappia perché è fatto così.** E quel qualcuno non esiste — non l'agente (che l'ha dimenticato alla sessione successiva), non tu (che non l'hai letto).

**I tre sintomi che riconosci quando ci sei dentro:**
1. Devi chiedere all'AI di spiegarti il tuo stesso sistema, e la risposta è plausibile ma non verificabile.
2. Ogni modifica richiede di rileggere tutto, perché non ti fidi delle tue assunzioni.
3. **Non sai dire se una funzionalità esiste già.** Quindi la fai riscrivere. Ora ce ne sono due.

**Le tre contromisure:**
1. **Decision Log** (→ A1): la teoria del sistema su disco.
2. **Confini netti** (→ A2): puoi capire un pezzo senza capire il tutto.
3. **Rifiuto attivo della complessità:** ogni astrazione, ogni livello, ogni "facciamolo generico" va **giustificato**. Il modello ne produce volentieri, gratis, e ogni pezzo gratuito ti costa comprensione.

> **La disciplina non è produrre meno codice. È produrre meno codice che non capisci.** La differenza è enorme, e non è la stessa cosa che rallentare.

---

## 4. Quando si ripaga il debito (e quando no)

**Il criterio è uno solo:**

> **Il debito si ripaga quando ti rallenta, non quando ti offende esteticamente.**

Codice brutto in un modulo che non tocchi da un anno e che funziona: **non è un debito, è codice brutto.** Lasciarlo lì è la decisione giusta. Rifattorizzarlo è vanità travestita da rigore, e introduce rischio a costo zero di beneficio.

**I segnali che il debito sta presentando il conto:**
- Modifiche semplici richiedono di toccare molti file (→ accoppiamento, A2).
- Hai paura di cambiare una certa area. **La paura è un dato**, non una debolezza: indica dove manca la rete di sicurezza.
- Ogni correzione ne genera un'altra altrove.
- Le stime che dai a te stesso sono sistematicamente sbagliate per difetto.
- I nuovi arrivati (o l'AI) non capiscono il sistema e fanno danni sistematici.

**La regola dei tre incontri:** quando **per la terza volta** un'area ti rallenta, quella è l'area da ripagare. Non prima (potrebbe non ricapitare), non dopo (stai già sanguinando).

---

## 5. La manutenzione: il ritmo minimo sostenibile

Il software "finito" non esiste, perché **il mondo continua a muoversi**: le dipendenze pubblicano falle di sicurezza, i fornitori cambiano le API, i browser cambiano comportamento, le norme cambiano.

**Il ritmo minimo, per un sistema piccolo in produzione: un'ora al mese.**

```
MANUTENZIONE MENSILE · 60 minuti

□ 15' — Dipendenze: aggiornamenti di sicurezza applicati, test verdi
□ 10' — Errori: guardo il tracciamento errori. Cosa si ripete? Cosa ignoro
        da troppo tempo?
□ 10' — Prestazioni: le richieste più lente sono peggiorate?
□  5' — Backup: l'ultimo è recente? (E ogni 6 mesi: PROVO il ripristino)
□ 10' — Debito: c'è un'area che mi ha rallentato tre volte questo mese?
□ 10' — Costi: la bolletta è quella prevista? Se è cresciuta, perché?
```

**Se questa ora non è in calendario, non accadrà.** E il costo di non farla non è lineare: si accumula in silenzio per mesi, e poi si presenta tutto insieme, sempre nel momento peggiore.

---

## 6. Riscrivere, o lasciar morire

Due decisioni che i tecnici prendono male per ragioni emotive, e che tu devi prendere da committente.

### La riscrittura completa
**Quasi sempre è un errore.** Il secondo sistema sembra sempre più semplice del primo, perché **hai dimenticato i mille casi particolari** che il primo gestisce e che sono la ragione per cui è complicato. La riscrittura parte veloce, arriva al 90% in fretta, e poi resta lì per un anno mentre riscopre uno a uno tutti i motivi per cui il vecchio era così brutto.

**Quando è giustificata:** il modello dati di fondo è sbagliato (→ A7), oppure il sistema è costruito su una tecnologia morta e insicura, e nessuno lo capisce più. Anche allora: **si riscrive un pezzo alla volta**, con i due sistemi che convivono, mai in un colpo solo.

### La dismissione
**Uccidere uno strumento è una decisione legittima, non un fallimento.** Anzi: è un segno di maturità. Ogni sistema che tieni vivo consuma manutenzione, attenzione, sicurezza — e **occupa il posto della cosa giusta**.

I criteri di morte vanno scritti nella Carta di progetto (→ A1) **quando sei lucido**, perché nel momento della decisione non lo sarai: sarai affezionato, e avrai il ricordo di quanto è costato costruirlo (che è, in economia, esattamente l'argomento da ignorare — il costo affondato non è un argomento).

**La dismissione fatta bene:** avvisa gli utenti con anticipo, esporta i loro dati in un formato che possono usare altrove, spegni. Un prodotto che muore con dignità lascia clienti che tornerebbero.

---

## Strumenti di questo modulo

1. **Le quattro specie di debito**, con rimedi diversi — inclusa quella nuova, "comprensione perduta".
2. **Il criterio di ripagamento:** rallenta, non offende.
3. **La regola dei tre incontri.**
4. **La checklist di manutenzione mensile** (60 minuti, in calendario).
5. **Il rifiuto attivo della complessità** come contromisura al debito generato.
6. **I criteri di morte**, scritti da lucidi.

---

## Errori tipici

1. **Chiamare debito ciò che è solo brutto.**
2. **Rifattorizzare per estetica**, introducendo rischio senza beneficio.
3. **Non aggiornare mai le dipendenze**, perché "funziona": funziona finché non è una falla nota, pubblica, e cercabile.
4. **Riscrivere da zero** per la fretta di liberarsi di qualcosa che non si è capito.
5. **Tenere in vita per orgoglio** ciò che nessuno usa.
6. **Accettare in silenzio codice che non capisci.** È l'unico modo in cui questo mestiere fallisce davvero.

*v1.0 — riscrittura in profondità.*
