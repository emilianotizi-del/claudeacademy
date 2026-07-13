# F5 — Lavorare con i documenti

**Codice modulo:** F5 · **Versione:** 0.1 · **Data:** 13/07/2026
**Dipendenze:** F1, F2, F3, F4, DATI-01 v1.0
**Richiamato da:** F6 · Corso 1 blocco 1.4 · Manuale M8, M9
**Etichette:** [FATTO] documentato · [BP] best practice consolidata · [OPINIONE] dibattuta · [SUGG] suggerimento del team

---

## 1. Scopo del modulo

Al termine di questo modulo saprai:

- scegliere **dove** caricare un documento (chat o conoscenza di Project) in base all'uso;
- padroneggiare i **quattro gesti** del lavoro sui documenti: estrarre, sintetizzare, confrontare, interrogare;
- gestire i **documenti difficili** (scansioni, Excel complessi, file molto lunghi) e verificare ciò che ne esce.

Perché conta: il lavoro sui *vostri* documenti è la zona a minor rischio di allucinazione (→ F1 §4.3): Claude non deve ricordare, deve **leggere**.

---

## 2. Dove caricare: chat o Project

**[BP]** La scelta dipende dalla vita del documento:

| | Allegato alla chat | Conoscenza del Project |
|---|---|---|
| **Uso** | Una tantum: "analizzami questo" | Permanente: serve a molte chat |
| **Esempi** | Il preventivo da confrontare, il paper da sintetizzare | Il programma dell'evento, la scheda dati, i template |
| **Manutenzione** | Nessuna | Va tenuta aggiornata (→ F4 §5.3) |

Prima di caricare qualsiasi cosa: il documento contiene dati personali o riservati? → i criteri sono in **F6** (in breve: si carica il necessario, anonimizzato quando possibile).

---

## 3. I quattro gesti

1. **Estrarre** — tirare fuori dati strutturati: "Dal programma in PDF estraimi la tabella: sessione, orario, relatori, moderatore."
2. **Sintetizzare** — ridurre preservando ciò che conta: verbali, paper, capitolati. La sintesi va **dimensionata sul destinatario** (§5).
3. **Confrontare** — due versioni o due documenti: "Confronta il programma v1 e v2: elenca ogni differenza di orario, titolo o relatore."
4. **Interrogare** — domande puntuali: "In questo contratto di sponsorizzazione, cosa è previsto in caso di annullamento dell'evento? Cita la clausola."

---

## 4. I casi B Best/HERA

**Il programma scientifico (blocco 1.4).**
```
Ti allego il programma preliminare di Romanestesia 2026. 1) Estrai la
griglia: giorno, orario, sessione, relatori, moderatori. 2) Segnala ogni
incongruenza: sovrapposizioni di orario, relatori in due sessioni
contemporanee, buchi tra sessioni, moderatori mancanti. 3) Per ogni
segnalazione, indica il punto esatto del documento.
```

**Il contratto di sponsorizzazione.**
```
Ti allego la bozza di contratto con [SPONSOR]. Estraine: importi e scadenze
di pagamento; deliverable a nostro carico con relative date; clausole di
recesso e annullamento. Poi preparami cinque domande da fare al nostro
consulente sui punti meno chiari, citando per ciascuna la clausola.
```
(Il parere resta al consulente: → F1 §5, → F6.)

**Il paper per la newsletter.**
```
Ti allego l'articolo. Sintetizzalo in 10 righe per la newsletter dei
partecipanti: pubblico di anestesisti, tono divulgativo ma rigoroso, niente
statistica inferenziale nel testo. Chiudi con "perché è rilevante per la
pratica clinica" in 2 righe.
```

---

## 5. Sintesi su misura

**[SUGG]** "Riassumi" è una richiesta nuda (→ F2). Le sintesi utili dichiarano destinatario e uso. Tre formati standard per l'azienda:

- **Executive** — 5 righe per la direzione: decisioni, impatti, scadenze; zero dettagli.
- **Operativa** — per chi deve agire: elenco azioni con responsabile e scadenza (il formato del caso Getinge di → F3 §6).
- **Tecnica** — per la segreteria scientifica: dettagli, dati e riferimenti conservati, con posizione nel documento.

---

## 6. Buone pratiche

1. **Chiedi la posizione.** "Per ogni informazione estratta, indica sezione e pagina": rende il controllo umano dieci volte più rapido. **[SUGG]** Regola aziendale: nessuna estrazione entra in un documento ufficiale senza controllo a campione sull'originale.
2. **Sui documenti lunghi, prima la mappa.** "Dammi l'indice ragionato del documento" e poi lavora sezione per sezione: meglio di una domanda generica su 80 pagine.
3. **Una domanda vaga su dieci file è la ricetta del disastro.** Carica il necessario e fai domande specifiche; se i file sono tanti e stabili, la loro casa è la conoscenza del Project (→ F4).
4. **I numeri estratti si verificano** (→ F3 §5): l'estrazione da tabelle e importi è affidabile ma non infallibile — e l'errore silenzioso sui numeri è il più costoso.
5. **Il risultato buono si deposita.** La griglia del programma appena validata può entrare nella conoscenza del Project: da lì in poi servirà a ogni chat.

---

## 7. Documenti difficili

- **Scansioni e PDF di sola immagine.** [BP] La qualità della lettura dipende dalla qualità della scansione: su documenti storti, timbrati o a bassa risoluzione aumenta l'errore di lettura. Contromisura: controllo a campione più fitto; se possibile, procurarsi il file nativo.
- **Excel complessi.** [BP] Fogli con formule, celle unite e più schede si prestano male alle domande generiche. Meglio: "Descrivimi prima la struttura del file: schede, colonne, cosa contiene ciascuna" e poi domande mirate su una scheda. Per i calcoli: fornire i numeri e chiedere la struttura, non delegare l'aritmetica (→ F1 §5).
- **Documenti molto lunghi.** Se il file eccede lo spazio disponibile, spezzalo per sezioni o mettilo nella conoscenza del Project, dove il recupero per pertinenza (→ F4 §3) fa da filtro. Per i limiti puntuali e aggiornati (dimensioni e formati) fa fede la documentazione ufficiale su support.claude.com.

---

## 8. Errori tipici

1. **Il caricamento-diluvio**: dieci file e una domanda vaga.
2. **Fidarsi delle tabelle ricostruite** senza controllo a campione.
3. **Chiedere la sintesi senza destinatario**: torna il §5.
4. **Lasciare in chat ciò che serve al progetto**: il risultato validato va depositato nella conoscenza.
5. **Caricare l'anagrafica completa quando serviva una colonna**: minimizzazione, → F6.
6. **Dimenticare che il documento batte la memoria**: se Claude contraddice il documento caricato, chiedigli di rileggere e citare il punto — non accettare la versione "a memoria".

---

## 9. Esercizio (blocco 1.4)

Sul programma scientifico fornito in aula: estrazione della griglia con posizioni, caccia alle incongruenze inserite ad arte dal docente, verifica a campione sull'originale, deposito della griglia corretta nella conoscenza del Project costruito nel blocco 2.2.

---

## 10. Punti chiave del modulo

1. Chat per l'una tantum, conoscenza di Project per ciò che dura.
2. Quattro gesti: estrarre, sintetizzare, confrontare, interrogare.
3. La sintesi dichiara destinatario e uso: executive, operativa, tecnica.
4. Chiedi sempre la posizione (sezione/pagina): il controllo diventa rapido.
5. Documenti lunghi: prima la mappa, poi le sezioni.
6. Scansioni ed Excel complessi: più struttura nelle domande, più controllo sull'output.
7. I numeri estratti si verificano; il documento batte la "memoria".
8. Prima di caricare: il filtro di → F6.

---

## 11. Collegamenti

- **← F1** (dove il rischio è basso) · **← F3** (verifica) · **← F4** (conoscenza di Project) · **→ F6** (cosa si può caricare)
- **Corso 1**: blocco 1.4 · **Manuale**: M8, M9

---

*Changelog: v0.1 — prima stesura; casi da DATI-01 v1.0.*
