# F2 — Scrivere buone richieste

**Codice modulo:** F2 · **Versione:** 0.1 · **Data:** 13/07/2026
**Dipendenze:** F1 (modello mentale), DATI-01 v1.0 (dati reali)
**Richiamato da:** F3, F4 · Corso 1 blocco 1.2 · Manuale M2 · Libreria prompt (Parte VII)
**Etichette:** [FATTO] documentato · [BP] best practice consolidata · [OPINIONE] dibattuta · [SUGG] suggerimento del team

---

## 1. Scopo del modulo

Al termine di questo modulo saprai:

- costruire richieste complete usando **i cinque elementi del briefing**;
- applicare **sei tecniche rapide** che alzano subito la qualità di qualsiasi risposta;
- trasformare i tuoi prompt migliori in **template riusabili** da tutto il team.

Prerequisito: il modello mentale di → F1. Se un risultato ti delude, il primo sospettato è il briefing — non Claude.

---

## 2. Il principio

> **Non esistono parole magiche: esiste un buon briefing.** Scrivere a Claude è spiegare bene un compito a un collaboratore appena arrivato (→ F1 §2).

**[SUGG]** Il test pratico: rileggi la tua richiesta e chiediti se una persona capace, ma appena assunta e senza accesso ai vostri archivi, potrebbe svolgerla bene. Se la risposta è no, manca qualcosa — e quasi sempre è uno dei cinque elementi qui sotto.

---

## 3. I cinque elementi del briefing

**[BP]** Le richieste efficaci contengono, in qualsiasi ordine, questi cinque ingredienti. Non servono tutti sempre; servono tutti quando il compito conta.

| Elemento | Che cosa dice | Esempio B Best/HERA |
|---|---|---|
| **Contesto** | Chi sei, di che evento parliamo, qual è la situazione | "Sei la segreteria organizzativa di Romanestesia 2026; Viatris è sponsor e non ha ancora inviato il logo" |
| **Obiettivo** | Cosa deve produrre Claude e a che cosa servirà | "Scrivi una mail di sollecito: dobbiamo ricevere il file entro una settimana" |
| **Formato** | Forma, lunghezza, lingua, tono dell'output | "Mail breve (max 120 parole), italiano, tono cordiale ma fermo" |
| **Vincoli** | Cosa includere per forza, cosa evitare, scadenze | "Non menzionare penali; ricorda che i materiali vanno in stampa il 30/10" |
| **Esempi** | Un modello di riferimento | "Ti allego una nostra mail di sollecito: usa quel tono" |

### 3.1 Il caso completo: il sollecito a Viatris

**V1 — richiesta nuda**

```
Scrivi una mail di sollecito a uno sponsor
```
→ mail generica, tono a caso, da rifare.

**V2 — con contesto e obiettivo**

```
Sei la segreteria organizzativa di Romanestesia 2026 (11–12 dicembre,
Auditorium Antonianum, Roma). Viatris, sponsor dell'evento, non ha ancora
inviato il logo per i materiali. Scrivi una mail di sollecito: dobbiamo
ricevere il file entro una settimana.
```
→ mail giusta, ma anonima nel tono e prolissa.

**V3 — completa (tutti e cinque gli elementi)**

```
Sei la segreteria organizzativa di Romanestesia 2026 (11–12 dicembre,
Auditorium Antonianum, Roma). Viatris, sponsor dell'evento, non ha ancora
inviato il logo per i materiali. Scrivi una mail di sollecito: dobbiamo
ricevere il file entro una settimana.
Formato: mail breve (max 120 parole), in italiano, tono cordiale ma fermo.
Non menzionare penali; ricorda che i materiali vanno in stampa il 30/10.
Firma: [nome], B Best – Segreteria Organizzativa.
Modello: la mail di sollecito che ti allego — usa quel tono.
```
→ bozza pronta al 90%.

**[SUGG]** Il briefing completo costa un minuto in più; risparmia decine di minuti di rilavorazione dopo.

---

## 4. Sei tecniche rapide

Tutte **[BP]**, salvo dove indicato.

1. **Assegna un ruolo.** "Agisci come un revisore madrelingua inglese di testi scientifici": attiva il registro giusto fin dalla prima riga. Funziona per revisioni, traduzioni, pareri tecnici.
2. **Chiedi le domande prima.** "Prima di rispondere, fammi le domande che ti servono per fare bene questo lavoro." **[SUGG]** È la tecnica più preziosa per chi inizia: Claude scova da solo il contesto che hai dimenticato di dare.
3. **Un compito per volta.** "Correggi la mail, poi traducila, poi preparami anche l'oggetto" produce tre risultati mediocri. Spezza: prima la correzione, la valuti, poi il resto (l'arte di concatenare i passi è in → F3).
4. **Di' cosa non vuoi.** "Niente elenchi puntati; non usare l'espressione «gentile dottore»; non superare le 120 parole." I divieti guidano quanto le richieste.
5. **Dai un esempio del risultato.** Allega una mail "come piace a noi", un verbale già ben sintetizzato, una scheda fatta bene: Claude ne assorbe tono e struttura meglio di qualsiasi descrizione.
6. **Chiedi varianti.** "Dammi tre versioni: formale, sintetica, persuasiva." Scegliere tra opzioni è più facile che descrivere in astratto ciò che vuoi. **[SUGG]** Ottimo anche per sbloccare i casi in cui "non sai cosa vuoi ma lo riconosci quando lo vedi".

Una settima tecnica circola molto: chiedere di "ragionare passo per passo". **[OPINIONE]** Sui compiti logici e di calcolo può aiutare; sui modelli recenti il ragionamento strutturato è in gran parte già incorporato, quindi il beneficio è discusso. Provala sui compiti analitici, non aspettarti miracoli altrove.

---

## 5. Prompt lungo o prompt corto?

**[OPINIONE]** Esistono due scuole, entrambe legittime:

| | Briefing completo subito | Partire snelli e iterare |
|---|---|---|
| **Quando** | Compiti ricorrenti, deliverable definiti, template | Esplorazione, brainstorming, quando non sai ancora cosa vuoi |
| **Vantaggi** | Meno iterazioni, risultati costanti, delegabile ai colleghi | La direzione emerge strada facendo; meno lavoro iniziale |
| **Limiti** | Sovradimensionato per richieste banali | Più botta e risposta; risultati meno riproducibili |

**[SUGG]** Regola pratica per B Best/HERA: se il compito lo rifarete (solleciti, convocazioni, sintesi verbali), investite nel briefing completo e trasformatelo in template (§7). Se state esplorando (titoli per una sessione, idee per uno sponsor), partite snelli. L'iterazione è il mestiere di → F3.

---

## 6. Gli otto errori del principiante

1. **"Sistemami questa mail"** e nient'altro: sistemarla per chi? in che lingua? con che tono?
2. **Contesto dato per scontato**: "prepara la lettera per il professore" — quale professore, quale evento, quale scopo?
3. **Tre compiti in una richiesta** (vedi tecnica 3).
4. **Nessun formato richiesto** → risposta fiume da cui estrarre a mano ciò che serve.
5. **Ripartire da zero a ogni modifica** invece di correggere in conversazione (→ F3).
6. **Fermarsi alla prima versione** senza chiedere alternative o critiche (→ F1 §4.4).
7. **Dati riservati incollati senza pensarci** — anagrafiche, dati sanitari, contratti: prima i criteri di → F6.
8. **Prompt "magici" copiati da internet** e incollati senza adattarli: un template altrui senza il vostro contesto è una richiesta nuda travestita bene.

---

## 7. Dal prompt al template: la libreria aziendale

**[SUGG]** Ogni prompt riuscito è un investimento: generalizzalo sostituendo i dettagli con campi tra parentesi quadre, e diventa un template che chiunque nel team può riusare.

**Anatomia di un template** (derivato dal caso Viatris):

```
Sei la segreteria organizzativa di [EVENTO]. Scrivi a [SPONSOR] un
sollecito per [DELIVERABLE], che ci serve entro il [DATA] per [MOTIVO].
Formato: mail breve (max 120 parole), italiano, tono cordiale ma fermo.
Vincoli: [COSA EVITARE / COSA RICORDARE].
Firma: [NOME], B Best – Segreteria Organizzativa.
```

**Altri due template di partenza per la libreria:**

*Convocazione faculty*
```
Prepara la mail di convocazione per la faculty di [EVENTO] ([DATE], [SEDE]).
Tono formale, in italiano. Chiedi conferma della disponibilità entro
[SCADENZA] e anticipa che la segreteria B Best contatterà i relatori per
l'organizzazione del viaggio. Alleghiamo [ALLEGATO].
```

*Sintesi di verbale*
```
Ti incollo il verbale della riunione [DATA/TEMA]. Sintetizzalo in:
1) decisioni prese, 2) azioni con responsabile e scadenza, 3) punti rinviati.
Massimo mezza pagina, elenchi puntati, italiano.
```

**Come vive la libreria:** un documento condiviso su Drive e, quando arriveremo a → F4, dentro il Project del team, così i template sono a portata di ogni chat. Nasce nel corso (blocco 2.5), cresce a ogni evento: chi crea un prompt che funziona, lo deposita.

---

## 8. Esercizi (tracce per ruolo — blocco 1.2)

- **Eventi**: mail a un fornitore di allestimenti: preventivo per la vela e il desk di Romanestesia 2026, sopralluogo possibile, risposta entro venerdì.
- **Segreteria scientifica**: tre bozze di domanda per il quiz ECM a partire da un abstract fornito in aula — con l'avvertenza che la validazione resta al responsabile scientifico.
- **Comunicazione**: post di annuncio dell'apertura iscrizioni, in due versioni: LinkedIn e newsletter.
- **Amministrazione**: mail di promemoria a Getinge per il pagamento della fattura di sponsorizzazione, cortese, con riferimenti [FATTURA] e [SCADENZA].

Per ogni traccia: prima scrivi la richiesta nuda, poi ricostruiscila con i cinque elementi, confronta i risultati.

---

## 9. Punti chiave del modulo

1. Briefing, non parole magiche.
2. Cinque elementi: contesto, obiettivo, formato, vincoli, esempi.
3. Le due leve più rapide: assegnare un ruolo e allegare un esempio.
4. "Fammi le domande prima": la tecnica salva-principianti.
5. Un compito per volta.
6. Lungo o corto: dipende dal compito, non dall'abitudine.
7. I prompt migliori diventano template della libreria aziendale.
8. Il passo successivo è iterare e verificare: → F3.

---

## 10. Collegamenti

- **← F1** Come ragiona Claude (il modello mentale che giustifica tutto questo)
- **→ F3** Iterare, correggere, verificare
- **→ F4** Contesto, chat e Projects (dove vivrà la libreria)
- **→ F6** Dati, riservatezza e responsabilità
- **Corso 1**: blocco 1.2 (il briefing perfetto) · **Manuale**: M2, Parte VII

---

*Changelog: v0.1 — prima stesura, esempi da DATI-01 v1.0 (Viatris, Getinge, Romanestesia 2026, quiz ECM).*
