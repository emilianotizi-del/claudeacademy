# F1 — Come ragiona Claude

**Codice modulo:** F1 · **Versione:** 0.2 · **Data:** 13/07/2026
**Dipendenze:** DATI-01 v1.0 (dati reali)
**Richiamato da:** F2, F3, F4, F5, F6 · Corso 1 blocchi 1.1 e 1.5 · Manuale M1
**Etichette:** [FATTO] documentato · [BP] best practice consolidata · [OPINIONE] dibattuta · [SUGG] suggerimento del team

---

## 1. Scopo del modulo

Al termine di questo modulo saprai:

- costruirti un **modello mentale corretto** di che tipo di "collaboratore" è Claude;
- prevedere **dove eccelle e dove sbaglia**, prima ancora di provarlo;
- riconoscere le **allucinazioni** e capire perché esistono (le tecniche per difenderti sono in → F3).

Non serve alcuna competenza tecnica. Non parleremo di come è fatto Claude "dentro" più di quanto serva per usarlo bene.

---

## 2. La frase da ricordare

> **Claude è un collaboratore brillante, colto e volenteroso, arrivato in azienda stamattina: non conosce B Best, non conosce HERA, non sa cos'è Romanestesia, non ha accesso ai vostri archivi — e per rendere bene ha bisogno di un buon briefing.**

**[SUGG]** Questa immagine guida tutto il percorso. Ogni volta che un risultato ti delude, chiediti: *"Un collaboratore nuovo, con il briefing che gli ho dato, avrebbe potuto fare meglio?"* Nella grande maggioranza dei casi la risposta è no — e la soluzione è migliorare il briefing (→ F2), non cambiare collaboratore.

---

## 3. Come funziona, in due minuti

**[FATTO]** Claude è un *modello linguistico di grandi dimensioni*: un sistema addestrato su enormi quantità di testo, che ha imparato i pattern del linguaggio — come si scrive una mail formale, come si struttura un programma scientifico, come si argomenta, come si traduce.

Tre conseguenze pratiche di questo funzionamento:

1. **Genera testo, non consulta un archivio.** Quando risponde, Claude *produce* la risposta parola per parola sulla base di ciò che ha imparato e di ciò che gli hai scritto. Non sta "cercando in un database" la risposta giusta.
2. **La sua conoscenza ha una data.** [FATTO] Il sapere appreso in addestramento si ferma a una certa data (che Claude sa dichiarare se glielo chiedi). Per informazioni recenti può usare la ricerca web, quando disponibile e attivata — ma va considerata uno strumento da verificare, non un oracolo (→ F3).
3. **Eccelle nella forma e nella trasformazione del linguaggio.** Riassumere, riscrivere, tradurre, adattare il tono, strutturare: sono i compiti in cui il suo funzionamento lo rende naturalmente fortissimo.

**[SUGG]** Non serve sapere altro sulla tecnologia. Tutto ciò che conta per il lavoro quotidiano discende da questi tre punti.

---

## 4. Le cinque conseguenze operative

### 4.1 Sa moltissimo del mondo, niente della tua azienda

Claude conosce l'anestesiologia, l'inglese scientifico, le regole generali dell'ECM italiano per come compaiono nei testi pubblici. **Non sa nulla** di Romanestesia 2026, di Hera Education, dei vostri sponsor, delle vostre procedure — finché non glielo dici tu, non glielo carichi (→ F5) o non lo trova nel Project (→ F4).

**Esempio.** Se chiedi *"prepara la mail di convocazione per la faculty"*, Claude inventerà un congresso plausibile ma sbagliato. Se scrivi:

> *"Prepara la mail di convocazione per la faculty di Romanestesia 2026 (XXII edizione), 11–12 dicembre, Auditorium Antonianum di Roma. Tono formale, in italiano. Chiediamo conferma della disponibilità entro due settimane e anticipiamo che la segreteria organizzativa B Best li contatterà per l'organizzazione del viaggio. Alleghiamo il programma preliminare."*

otterrai una bozza quasi pronta.

### 4.2 Ogni chat parte (quasi) da zero

**[FATTO]** Una nuova conversazione non "ricorda" automaticamente le precedenti nel senso pieno del termine. Esistono strumenti che attenuano questo limite — i Projects e le funzioni di memoria — e li tratteremo in → F4. La regola prudente resta: **ciò che non è nella chat, nei documenti caricati o nel Project, per Claude non esiste.**

### 4.3 Può sbagliare con totale sicurezza (le "allucinazioni")

**[FATTO]** Un'allucinazione è un'informazione inventata ma presentata con tono sicuro: un riferimento bibliografico inesistente, una scadenza ECM sbagliata, l'affiliazione errata di un relatore, un dato numerico plausibile ma falso.

Perché succede? Perché Claude genera il testo *più plausibile*, e a volte il testo più plausibile non è quello vero. Non sta "mentendo": non distingue, mentre scrive, tra ciò che ricorda bene e ciò che sta ricostruendo male.

**Quando il rischio è più alto** [BP]:

- nomi propri, date, numeri, riferimenti bibliografici, normative specifiche (es. requisiti Agenas per l'accreditamento ECM);
- domande molto specifiche su fatti di nicchia o recenti;
- quando *tu stesso* dai per scontato un fatto sbagliato nel prompt (Claude tende ad assecondarti);
- richieste dove "non lo so" sarebbe la risposta onesta ma il compito spinge a rispondere comunque.

**Quando il rischio è basso**: trasformazioni di testo che gli fornisci tu (riassumere il *tuo* verbale, tradurre la *tua* mail, riscrivere il *tuo* paragrafo). Qui Claude lavora su materiale presente, non deve "ricordare" nulla.

**[SUGG]** Regola aziendale che proporremo come standard: *ogni nome, data, numero e riferimento normativo in un output di Claude va verificato prima di uscire da B Best/HERA.* Un'affiliazione sbagliata in una mail alla faculty o una data errata in una comunicazione agli sponsor costano più del tempo di verifica. Le tecniche pratiche sono in → F3; la responsabilità sull'output firmato è in → F6.

### 4.4 Asseconda volentieri — anche quando non dovrebbe

**[BP]** I modelli linguistici tendono ad andare incontro all'interlocutore: se chiedi *"perché questa bozza di programma è ottima?"* troverà ragioni per cui è ottima. Per un giudizio utile, chiedi esplicitamente il contrario: *"trova i tre problemi principali di questa bozza"* oppure *"fai l'avvocato del diavolo: cosa obietterebbe un anestesista esperto a questo programma?"*. (→ F3 per le tecniche di revisione critica.)

### 4.5 Il risultato dipende dal briefing più che dalla domanda

La stessa richiesta, formulata con o senza contesto, produce risultati radicalmente diversi. Questo è il cuore di → F2; qui basti l'evidenza:

| Richiesta | Risultato tipico |
|---|---|
| "Scrivi una mail allo sponsor" | Mail generica, tono a caso, da rifare |
| "Scrivi una mail a Getinge, major sponsor di Romanestesia 2026: hanno confermato la sponsorizzazione, dobbiamo sollecitare con garbo l'invio dei deliverable (logo vettoriale, grafica per la vela) entro fine ottobre per l'avvio della stampa. Tono cordiale ma fermo. Firma: [nome], B Best – Segreteria Organizzativa" | Bozza pronta al 90% |

---

## 5. Dove eccelle / dove serve prudenza

### Punti di forza (usali senza esitazione)

| Attività | Esempio B Best/HERA |
|---|---|
| Riscrivere e adattare il tono | Trasformare appunti in una comunicazione formale ai responsabili scientifici di Romanestesia |
| Sintetizzare | Verbale di riunione di 6 pagine → mezza pagina di decisioni e azioni |
| Tradurre e revisionare | Mail a un relatore straniero: da "inglese scolastico" a inglese professionale |
| Strutturare | Da un elenco disordinato di interventi a una griglia di programma su due giornate |
| Analizzare documenti forniti | Trovare incongruenze di orari o moderatori duplicati nel programma in PDF (→ F5) |
| Fare brainstorming | Dieci titoli alternativi per una sessione; possibili obiezioni di Viatris al posizionamento dello spazio espositivo |
| Preparare prime bozze | Bozza di Sponsor Prospectus, procedure interne, comunicati, FAQ per i partecipanti, **prime bozze di domande per i quiz ECM** (da validare sempre dal responsabile scientifico) |

### Zone di prudenza (usalo, ma con verifica sistematica)

| Attività | Rischio | Contromisura |
|---|---|---|
| Fatti specifici e recenti | Allucinazioni | Verifica su fonte primaria (→ F3) |
| Normativa ECM e adempimenti Agenas | Dettagli datati o inventati | Documento normativo ufficiale come fonte, caricato o citato |
| Bibliografia e citazioni scientifiche | Riferimenti inventati | Mai usare citazioni non verificate una per una |
| Calcoli lunghi e tabelle numeriche | Errori silenziosi | Ricontrollo, o fornire i numeri e chiedere solo la struttura |
| Giudizi su persone reali | Confusioni tra omonimi, dati datati | Non usare Claude come "dossier" su relatori o clienti |

### Cosa non chiedergli affatto

- Decisioni che spettano a un responsabile (Claude prepara opzioni e analisi; **decide una persona**: es. accettare o meno le condizioni di uno sponsor).
- Pareri legali o fiscali definitivi: può aiutare a *capire* un contratto di sponsorizzazione e a *preparare domande per il consulente*, non a sostituirlo.
- Qualsiasi compito che richieda dati che non puoi caricarvi (→ F6 per i criteri su dati personali, sanitari e riservati).

---

## 6. Il modello mentale, riassunto in tre confronti

**[SUGG]** Tre paragoni per fissare le idee — ognuno corregge un'aspettativa sbagliata frequente:

1. **Non è Google.** Google trova documenti esistenti; Claude genera testo nuovo. Per "trovare la fonte" usa la ricerca (web o d'archivio); per "lavorare sul testo" usa Claude. I due gesti sono diversi e complementari.
2. **Non è un software gestionale.** Hera Education o Fattureincloud danno sempre la stessa risposta agli stessi input; Claude no, e formulazioni diverse producono risultati diversi. È un difetto? No: è ciò che gli permette di adattarsi — ma richiede il tuo controllo.
3. **Non è un collega esperto della vostra storia.** È il collaboratore brillante del §2: rende in proporzione al briefing. La buona notizia: il briefing si impara in fretta, ed è esattamente l'oggetto di → F2.

---

## 7. Punti chiave del modulo

1. Claude **genera** testo plausibile; non consulta un archivio di verità.
2. Sa moltissimo del mondo, **niente di B Best/HERA o di Romanestesia**: il contesto glielo dai tu.
3. Ogni chat parte da zero, salvo ciò che metti nel Project (→ F4).
4. Le allucinazioni esistono, sono prevedibili e si gestiscono: massima allerta su **nomi, date, numeri, norme, citazioni**.
5. Tende ad assecondarti: per giudizi utili, chiedi la critica esplicitamente.
6. È fortissimo nel **trasformare** testo che gli fornisci; è lì che conviene partire.
7. La qualità dell'output è funzione della qualità del **briefing** (→ F2).

---

## 8. Mini-glossario del modulo

| Termine | Definizione operativa |
|---|---|
| Modello linguistico | Sistema addestrato su testi che genera linguaggio prevedendo la continuazione più plausibile |
| Prompt | La richiesta (con tutto il suo contesto) che scrivi a Claude |
| Contesto | Tutto ciò che Claude "ha davanti" in quel momento: la conversazione, i documenti caricati, le istruzioni del Project (→ F4) |
| Allucinazione | Informazione inventata presentata con sicurezza |
| Data di conoscenza (cutoff) | Data oltre la quale il sapere appreso in addestramento non arriva |

Il glossario completo del percorso è nel Manuale, modulo M21.

---

## 9. Collegamenti

- **→ F2** Scrivere buone richieste (il briefing in pratica)
- **→ F3** Iterare, correggere, verificare (tecniche anti-allucinazione)
- **→ F4** Contesto, chat e Projects (la "memoria" organizzata)
- **→ F5** Lavorare con i documenti
- **→ F6** Dati, riservatezza e responsabilità
- **Corso 1**: questo modulo alimenta i blocchi 1.1 (demo mail in inglese) e 1.5 (caccia all'allucinazione)

---

*Changelog:*
*v0.2 — sostituiti tutti i segnaposto con i dati reali di DATI-01 v1.0 (Romanestesia 2026, Getinge/Viatris, Hera Education, quiz ECM); aggiunti riferimenti ad Agenas e al ciclo deliverable sponsor.*
*v0.1 — prima stesura con segnaposto.*
