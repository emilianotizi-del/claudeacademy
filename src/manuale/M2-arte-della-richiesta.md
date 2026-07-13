# M2 — L'arte della richiesta: prompt annotati e pattern

**Codice:** M2 · **Versione:** 0.1 · 13/07/2026 · **Estende:** F2

## 1. Tre coppie annotate (cattivo → buono)

### A. La mail alla faculty
❌ `Scrivi una mail al professore per il congresso`
→ *quale professore? quale congresso? per dirgli cosa?* — tre buchi di contesto in nove parole.

✅ Il prompt della demo d'aula (AULA-02, passo 3): destinataria identificata, tema proposto, scadenza, chi cura la logistica, tono, lingua, lunghezza, firma. **Nota il dettaglio che fa la differenza:** "relatrice apprezzata alla scorsa edizione" — una riga di relazione che cambia il calore della mail.

### B. Il preventivo al fornitore
❌ `Chiedi un preventivo per il congresso`
✅ `Scrivi a [FORNITORE] per un preventivo: vela esterna 3x1 m e desk reception per Romanestesia 2026 (Auditorium Antonianum, Roma, 11–12 dicembre; montaggio il 10 pomeriggio). Chiedi: costo, tempi di produzione, formato file grafico richiesto, possibilità di sopralluogo. Risposta entro venerdì. Tono professionale asciutto.`
**Perché funziona:** le domande esplicite (costo/tempi/formato/sopralluogo) trasformano una richiesta generica in una checklist che il fornitore può evadere in una risposta sola.

### C. Il post social
❌ `Scrivi un post sull'apertura delle iscrizioni`
✅ `Post LinkedIn per l'apertura iscrizioni di Romanestesia 2026 (11–12 dicembre, Roma). Pubblico: anestesisti e rianimatori. Leva: quota early per le prime 100 iscrizioni. Includi: date, sede, link [LINK]. Tono professionale con un tocco di orgoglio per la XXII edizione. 3 versioni: sobria, energica, istituzionale. Niente hashtag oltre a 3.`
**Perché funziona:** pubblico + leva + varianti = si sceglie invece di descrivere.

## 2. Pattern oltre i cinque elementi

- **A scaletta:** "Prima propone la struttura in punti; quando la approvo, scrivi il testo." Su documenti lunghi evita di buttare via pagine intere.
- **L'esempio negativo:** allegare anche una mail "come NON piace a noi" e dire perché. I divieti concreti insegnano più delle regole astratte.
- **Il doppio registro:** "Stesso contenuto in due versioni: per la faculty (formale) e per il gruppo interno (schietta)." Un lavoro, due output.
- **La domanda di riserva:** chiudere il prompt con "se qualcosa è ambiguo, chiedi invece di assumere" — l'assunzione silenziosa è la madre delle bozze sbagliate.

## 3. Scrivere template robusti (per la libreria)

1. I campi si scrivono `[NOME CAMPO: cosa metterci]` — es. `[SCADENZA: data entro cui serve il file]`: chi compila non deve indovinare.
2. Ogni template porta in coda un **esempio di output riuscito**: taratura immediata.
3. Un template = un compito. "Template mail sponsor" è troppo largo; "sollecito deliverable" è giusto.
4. Versione e owner in testa (come i moduli di questo manuale): i template invecchiano.

*Changelog: v0.1 — prima stesura; coppie annotate su casi DATI-01/AULA.*
