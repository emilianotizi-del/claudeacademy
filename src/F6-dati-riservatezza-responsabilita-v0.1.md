# F6 — Dati, riservatezza e responsabilità

**Codice modulo:** F6 · **Versione:** 0.1 · **Data:** 13/07/2026
**Dipendenze:** F1, F4, F5, DATI-01 v1.0
**Richiamato da:** tutti i moduli operativi · Corso 1 blocco 2.4 · Manuale M15, M16
**Etichette:** [FATTO] documentato · [BP] best practice consolidata · [OPINIONE] dibattuta · [SUGG] suggerimento del team
**Fonti verificate il 13/07/2026:** privacy.claude.com ("Is my data used for model training?"), anthropic.com/news ("Updates to Consumer Terms"). Le policy evolvono: fa fede la versione corrente delle fonti ufficiali. Questo modulo non è un parere legale: le decisioni di conformità GDPR spettano alla direzione con il proprio consulente.

---

## 1. Scopo del modulo

Al termine di questo modulo saprai:

- classificare qualsiasi contenuto con il **semaforo dei dati** prima di caricarlo;
- applicare le **tecniche di minimizzazione** che rendono usabile Claude anche su casi delicati;
- che cosa prevede il **piano aziendale** sul trattamento dei dati, e dove finisce la sua protezione e comincia la vostra **responsabilità**.

---

## 2. Perché per noi il tema è serio

B Best e HERA lavorano ogni giorno con: **anagrafiche** di faculty e iscritti (nomi, contatti, dati di viaggio), materiale **ECM** in un contesto sanitario, **contratti** con sponsor farmaceutici e di dispositivi medici. Un uso ingenuo di Claude su questi materiali espone a violazioni GDPR, danni reputazionali con sponsor e provider, e violazioni di clausole di riservatezza. La buona notizia: con tre regole semplici, il 95% del lavoro quotidiano resta possibile e sicuro.

---

## 3. Il semaforo dei dati

**[SUGG]** Prima di incollare o caricare qualsiasi cosa, un secondo di classificazione:

### 🟢 Verde — si carica senza esitazione
Contenuti pubblici o interni senza dati personali: programmi scientifici, testi da revisionare (senza dati personali), procedure interne, template, comunicazioni già pubbliche, materiali promozionali.

### 🟡 Giallo — solo il necessario, e con giudizio
Dati personali "comuni" quando servono davvero al compito: nome e affiliazione di un relatore per scrivergli una mail, la città di partenza per organizzare un viaggio. Regola: **minimizzare** (§4) — serve la città della prof.ssa Battaglini, non il suo codice fiscale.

### 🔴 Rosso — non si carica; si anonimizza o si fa senza Claude
- **Dati sanitari di persone** e categorie particolari GDPR (salute, dati genetici/biometrici, orientamento, opinioni…): un elenco iscritti è giallo; lo stesso elenco con patologie o esenzioni sanitarie è rosso.
- **Credenziali**: password, chiavi, codici di accesso (Hera Education, Fattureincloud, home banking) — mai, da nessuna parte.
- **Dati di pagamento** completi: IBAN di terzi, carte.
- **Documenti coperti da NDA** o clausole di riservatezza esplicite, salvo valutazione della direzione.
- **Dati di minori.**

**[SUGG]** In caso di dubbio tra giallo e rosso: tratta come rosso e chiedi alla direzione. Il dubbio costa un messaggio; l'errore costa una violazione.

---

## 4. Minimizzare: le tre tecniche

1. **Anonimizza o pseudonimizza.** "Prepara la mail di rifiuto per [RELATORE A], che ha chiesto un rimborso fuori policy": il nome vero lo inserisci tu dopo, nel tuo client di posta. Per i casi ripetitivi, i template di → F2 §7 nascono già anonimi.
2. **Togli le colonne che non servono.** Dall'Excel iscritti serve la colonna "professione" per le statistiche ECM? Esporta quella, non il file con email e telefoni (→ F5).
3. **Costruisci sul finto, compila col vero.** Fai costruire a Claude la struttura (lettera, tabella, procedura) su dati fittizi realistici; i dati veri li inserisci a mano nel documento finale.

---

## 5. Cosa prevede il piano aziendale

**[FATTO]** B Best/HERA usa un piano **Claude for Work (Team/Enterprise)**, coperto dai termini commerciali di Anthropic: per questi piani, **input e output non vengono usati per addestrare i modelli** per impostazione predefinita. Un'eccezione va conosciuta: il **feedback volontario** (pulsanti 👍/👎) invia ad Anthropic la conversazione collegata; gli amministratori dell'organizzazione possono disabilitare questa funzione. Fonte: privacy.claude.com, "Is my data used for model training?" — da riverificare periodicamente perché le policy evolvono.

**[SUGG]** Tre conseguenze pratiche:

1. **Usate solo l'account aziendale** per il lavoro: un account personale gratuito o Pro ha condizioni diverse (termini consumer, con impostazioni di training da gestire individualmente). Il "shadow AI" — lavoro aziendale su account personali — è il rischio numero uno.
2. **Niente 👍/👎 su conversazioni con dati aziendali**, finché la direzione non decide una linea (o disabilita la funzione).
3. La protezione contrattuale **non trasforma il rosso in verde**: che Anthropic non addestri sui vostri dati non autorizza a caricare dati sanitari o credenziali. Il semaforo resta.

---

## 6. La responsabilità resta umana

**[SUGG]** Tre regole che proporremo come standard aziendale:

1. **L'output firmato è di chi lo firma.** La mail alla faculty, la sintesi alla direzione, il testo per lo sponsor: chi invia risponde del contenuto, verifiche incluse (→ F3 §5).
2. **Le bozze di quiz ECM sono bozze**: la validazione scientifica spetta sempre al responsabile scientifico di Hera Foundation, e la dicitura "da validare" accompagna ogni bozza (già nelle istruzioni del Project, → F4 §5.2).
3. **Le decisioni le prendono le persone** (→ F1 §5): Claude prepara opzioni e analisi; accettare le condizioni di uno sponsor, rispondere a una contestazione, definire una policy sono atti umani.

---

## 7. La policy aziendale proposta (bozza da adottare)

**[SUGG]** Sei punti, una pagina, da far approvare alla direzione e allegare al manuale:

1. Per il lavoro si usa **solo l'account Claude aziendale**.
2. Prima di caricare: **semaforo** (§3). Nel dubbio: rosso → si chiede.
3. **Mai** credenziali, dati sanitari di persone, dati di pagamento completi, dati di minori.
4. Dati personali comuni: **solo se necessari** al compito, minimizzati (§4).
5. **Ogni output verso l'esterno è verificato e firmato** da una persona; nomi, date, numeri e norme si controllano sempre (→ F3).
6. La **conoscenza dei Project condivisi** contiene solo materiale verde o giallo-minimizzato; l'owner del Project ne risponde (→ F4 §6).

---

## 8. Il semaforo alla prova: dieci casi reali

| Caso | Colore | Perché |
|---|---|---|
| Testo di una mail da rendere più formale (senza dati personali) | 🟢 | Nessun dato personale |
| Programma scientifico di Romanestesia | 🟢 | Destinato alla pubblicazione |
| Procedura interna "avvio evento su Hera Education" | 🟢 | Interna, non personale |
| Abstract scientifico senza autori | 🟢 | Contenuto tecnico |
| Nome + affiliazione di un relatore per una convocazione | 🟡 | Personale comune, necessario al compito |
| Città e date di viaggio della faculty per la logistica | 🟡 | Necessario, minimizzato |
| Elenco iscritti completo con email e telefoni | 🟡/🔴 | Solo colonne necessarie; mai in Project condiviso |
| Contratto Getinge con clausola di riservatezza | 🔴 | NDA: valutazione della direzione |
| Elenco con esenzioni o dati sanitari dei partecipanti | 🔴 | Categoria particolare GDPR |
| Credenziali Hera Education / Fattureincloud | 🔴 | Mai, in nessun caso |

---

## 9. Errori tipici

1. **"Tanto è il piano aziendale, posso caricare tutto"** — la protezione contrattuale non elimina il semaforo (§5.3).
2. **Lavoro aziendale sull'account personale** — condizioni diverse, controllo zero.
3. **L'anagrafica intera "per comodità"** quando serviva una colonna.
4. **Il contratto con NDA caricato "solo per una sintesi"**.
5. **Dati sensibili nella conoscenza di un Project condiviso** — visibili a chiunque abbia accesso, anche domani.
6. **Il 👍 di cortesia** su una conversazione piena di dati aziendali.

---

## 10. Esercizio (blocco 2.4)

Il semaforo alla prova: 12 casi reali proposti dal docente (varianti del §8), classificazione individuale, confronto in aula, e per i gialli: come minimizzare in pratica. Chiusura: lettura e discussione della policy del §7.

---

## 11. Punti chiave del modulo

1. Semaforo prima di ogni caricamento: verde / giallo minimizzato / rosso mai.
2. Rosso non negoziabile: dati sanitari di persone, credenziali, pagamenti, NDA, minori.
3. Minimizzare: anonimizza, togli colonne, costruisci sul finto.
4. Piano Team/Enterprise: niente addestramento sui vostri dati per default — ma il feedback 👍/👎 fa eccezione, e il semaforo resta.
5. Solo account aziendale per il lavoro.
6. L'output firmato è di chi lo firma; le bozze ECM si validano; le decisioni sono umane.
7. La policy in sei punti (§7) va adottata dalla direzione.
8. Nel dubbio: rosso, e si chiede.

---

## 12. Collegamenti

- **← F1** (cosa non chiedergli) · **← F3** (verifica) · **← F4** (Project condivisi) · **← F5** (documenti e minimizzazione)
- **Corso 1**: blocco 2.4 · **Manuale**: M15, M16

---

*Changelog: v0.1 — prima stesura; policy dati verificata su privacy.claude.com e anthropic.com/news il 13/07/2026; casi da DATI-01 v1.0.*
