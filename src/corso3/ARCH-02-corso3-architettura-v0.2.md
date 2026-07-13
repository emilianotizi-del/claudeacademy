# ARCH-02 — Corso 3 · AI Software Architect — architettura (progetto: Giano)

**Codice:** ARCH-02 · **Versione:** 0.2 · 13/07/2026 · sostituisce v0.1 (progetto Sponsor Tracker)
**Destinatario unico:** direzione (Emiliano Tizi) · **Prerequisito:** F1–F6
**Scelte confermate:** progetto = **Giano** · ritmo = **modulo → esecuzione → modulo successivo** · ambizione = **potenzialmente un prodotto**

---

## 1. Il progetto: Giano

**Piattaforma gestionale per società scientifiche, associazioni professionali e fondazioni.** Centralizza soci, quote, rinnovi, cariche, gruppi di studio e sezioni territoriali; gestisce assemblee, candidature, deleghe, votazioni, patrocini, bandi e documenti istituzionali. Si integra con LMS, Aurora e Madeira. Obiettivo: digitalizzare la governance associativa in modo semplice, trasparente e tracciabile.

### 1.1 Che tipo di sistema è (e perché conta saperlo subito)

Giano è un **sistema di registro**: contiene la verità su chi è socio, chi è in regola, chi può votare, chi ha vinto un'elezione. I sistemi di registro hanno tre proprietà che li rendono diversi da un cruscotto interno:

1. **Le loro decisioni hanno effetti giuridici.** Un elenco soci sbagliato invalida un'assemblea. Un voto perso è contestabile in tribunale.
2. **I dati sono personali e permanenti.** Nomi, contatti, quote pagate, appartenenze — dati di categoria comune, ma in volume e con conservazione lunga.
3. **Hanno molti utenti che non conosci.** I soci di un'associazione cliente non sono i tuoi otto colleghi: useranno il sistema in modi che non prevedi.

**Conseguenza operativa per il corso:** Giano non è "un'app da costruire in un pomeriggio". Il corso ti insegnerà a **dirigerne lo sviluppo**, e a farlo per fasi — costruendo tu una parte, e sapendo con precisione dove passare il testimone a professionisti.

---

## 2. Il confine, scritto prima di iniziare

**[SUGG — ma raccomandato con forza]** Ci sono tre aree di Giano dove il fai-da-te, anche assistito da Claude, è **irresponsabile**:

| Area | Perché | Cosa fare |
|---|---|---|
| **Votazioni e deleghe** | Un'elezione contestata è un contenzioso. Servono garanzie di integrità, segretezza dove prevista, tracciabilità, resistenza alle contestazioni. Non è un problema di codice: è un problema di garanzie dimostrabili. | Sviluppo professionale + revisione legale dello statuto-tipo. Tu la specifichi, non la implementi. |
| **Pagamenti (quote e rinnovi)** | Toccare dati di carte o flussi di incasso significa obblighi di conformità (PCI-DSS, antiriciclaggio a seconda dei casi). | Ci si appoggia a un fornitore di pagamenti certificato; Giano registra l'esito, non tratta la carta. |
| **Dati personali di terzi su larga scala** | Non sono più i vostri dati: sono i dati dei soci dei vostri clienti. Voi diventate responsabile del trattamento per conto loro. | DPO/consulente privacy, DPA con i clienti, misure di sicurezza documentate. |

**Ciò che invece puoi legittimamente dirigere tu, con questo corso:** il modello dei dati, l'anagrafica soci e le sue regole, i rinnovi e gli stati di iscrizione, le cariche e i mandati, i gruppi di studio e le sezioni, i documenti istituzionali, la reportistica, le integrazioni. **È la maggior parte di Giano.**

> **La regola d'oro del corso:** *tu sei l'architetto e il committente. Le fondamenta rischiose le fa chi ha licenza per farle — ma la casa la disegni tu, e sai riconoscere se è stata costruita male.*

---

## 3. La spina dorsale: una fetta verticale, non tutto Giano

**[BP]** Il modo in cui i prodotti seri nascono non è "costruiamo tutto e poi lanciamo": è **una fetta verticale** — un pezzo completo che funziona davvero, dall'interfaccia al database — che poi si allarga.

**La fetta scelta per il corso: "Soci, quote e rinnovi".**

Perché è la fetta giusta:
- **È il cuore:** senza anagrafica soci affidabile, nulla del resto di Giano ha senso (chi vota? chi è in regola? chi riceve il bando?).
- **È autosufficiente:** un'associazione la userebbe da subito, anche senza il resto.
- **Non tocca le tre aree proibite** (voto, carte di credito, ma solo la registrazione degli esiti).
- **Insegna tutto ciò che serve:** modello dati, ruoli e permessi, interfaccia, importazione da Excel, scadenze, comunicazioni, report.
- **Prepara il terreno:** quando arriveranno le assemblee, l'elenco degli aventi diritto sarà già lì, corretto.

Le altre aree (assemblee, votazioni, bandi, patrocini) restano nella **specifica di prodotto** — le progetti, non le costruisci ora.

---

## 4. Struttura: cinque parti, quindici moduli

### PARTE A — Il mestiere del committente tecnico (A1–A3)
- **A1 · Dirigere lo sviluppo.** Il tuo ruolo reale (committente + architetto), le tre domande prima di ogni riga di codice, e il confine del §2 approfondito. Deliverable: **la carta del progetto Giano** (una pagina: cosa è, cosa non è, cosa non farai da solo).
- **A2 · Il vocabolario minimo.** Trenta termini per analogia: frontend, backend, database, tabella, relazione, API, repository, commit, branch, deploy, ambiente, autenticazione, autorizzazione, migrazione, multi-tenancy. Serve a non essere raggirabile e a scrivere richieste eseguibili.
- **A3 · Specificare invece di programmare.** Requisiti, casi d'uso, criteri di accettazione, fuori-scopo esplicito. Deliverable: **la specifica della fetta "Soci, quote e rinnovi"** — il documento che guiderà tutta la Parte C.

### PARTE B — Gli strumenti (A4–A6)
- **A4 · Claude Code: l'esecutore.** Sessioni di lavoro, file di istruzioni permanente del progetto, come si legge ciò che ha fatto. Deliverable: **ambiente funzionante**.
- **A5 · GitHub: la memoria e la rete di sicurezza.** Commit, cronologia, ripristino, branch. Gestione dei segreti (il caso del token di questo percorso come esempio didattico). Deliverable: **repository Giano** con la specifica dentro.
- **A6 · Supabase e Vercel: dove vive.** Database e hosting spiegati; ambienti separati (sviluppo/produzione) — la regola che impedisce di distruggere dati veri sperimentando. **Con l'ambizione-prodotto: introduzione alla separazione dei dati per cliente (multi-tenancy), da decidere ORA e non dopo.**

### PARTE C — Costruire la fetta (A7–A11)
- **A7 · Dalla specifica al prototipo.** Primo ciclo completo. Deliverable: **prototipo navigabile della gestione soci**.
- **A8 · Il modello dei dati.** Soci, quote, rinnovi, cariche, sezioni, gruppi: tabelle e relazioni. **Il modulo che decide il destino del prodotto:** un modello sbagliato si paga per anni. Deliverable: **modello dati rivisto e approvato** (con una revisione esterna consigliata).
- **A9 · Ruoli, permessi e interfaccia.** Chi vede cosa: segreteria, presidente, socio, tesoriere. L'interfaccia che i soci useranno senza istruzioni. Deliverable: **versione usabile**.
- **A10 · Verificare il codice che non hai scritto.** Il modulo più importante: revisione avversariale, test automatici spiegati al committente, controllo dei dati personali, lettura del diff, seconda opinione. *Funziona ≠ è corretto.*
- **A11 · Dalla prova alla produzione.** Importazione dei dati reali da Excel (il momento più pericoloso), prima associazione pilota, checklist di lancio.

### PARTE D — Governare un prodotto (A12–A14)
- **A12 · Manutenzione, debito tecnico, evoluzione.** Il ritmo minimo; quando riscrivere; come si accolgono le richieste dei clienti senza far esplodere il sistema.
- **A13 · Sicurezza, privacy e conformità — sul serio.** Con l'ambizione-prodotto questo modulo diventa **vincolante**: ruolo di responsabile del trattamento, DPA con i clienti, backup, cifratura, registro dei trattamenti, cosa chiedere al consulente. Include il confine del §2, in versione operativa.
- **A14 · Costi, dipendenze, sostenibilità.** Quanto costa per associazione; cosa succede se un fornitore cambia prezzi; **e se tu non ci fossi? chi lo mantiene?** — documentazione, exit plan, e la domanda onesta: *quando conviene comprare invece di costruire?*

### PARTE E — Il metodo (A15)
- **A15 · Il ciclo, generalizzato.** Dal problema alla produzione, ripetibile. La griglia delle 8 domande per decidere cosa costruire e cosa no — utile per Aurora, Madeira, LMS e per ciò che verrà. Chiusura: **quando smettere di fare da soli** — come si sceglie e si dirige un fornitore tecnico, ora che sai cosa chiedere.

---

## 5. Il ritmo (confermato)

Modulo → **tu esegui la tappa** → mi riporti cosa è successo → modulo successivo, arricchito da ciò che hai incontrato davvero. Ogni modulo ha: *obiettivo · concetti · prompt operativi pronti · deliverable · come verificare di averlo fatto bene · errori tipici*.

Materiali: sorgenti md nel repo · sezione `architect.html` sul sito · **DIARIO-01.md**, il tuo diario di bordo (è anche ciò che permetterà domani a un tecnico esterno di raccapezzarsi).

---

## 6. Cosa mi serve sapere prima di A2 (non blocca A1)

1. **Aurora, Madeira, LMS**: cosa sono, chi li ha costruiti, esistono già o sono progetti come Giano? (L'integrazione con sistemi che non esistono ancora si progetta in modo diverso.)
2. **Esiste già qualcosa di Giano?** Documento, prototipo, un preventivo ricevuto da un fornitore, un Excel che oggi fa questo lavoro?
3. **Chi altro c'è?** Sei solo tu, o c'è già uno sviluppatore/fornitore con cui lavori?
4. **Il primo cliente**: c'è una società scientifica reale che lo userebbe (anche solo come pilota)? Avere un pilota vero cambia tutto in meglio.

---

*Changelog: v0.2 — progetto Giano; aggiunti confine di responsabilità (§2) e strategia a fetta verticale (§3). v0.1 — proposta su Sponsor Tracker, superata.*
