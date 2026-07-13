# M22 — Tenere vivo il percorso: versioni e aggiornamenti

**Codice:** M22 · **Versione:** 0.1 · 13/07/2026 · Parte VII · come questo manuale resta vero nel tempo
Un manuale sull’AI invecchia più in fretta di qualsiasi altro. Questo modulo definisce **chi lo aggiorna, quando e come** — altrimenti tra un anno sarà un documento che nessuno apre perché nessuno si fida.

## Le tre categorie di contenuto (invecchiano a velocità diverse)

| Categoria | Esempi | Frequenza di revisione |
|---|---|---|
| **Principi** | Briefing, iterazione, verifica, semaforo | Rarissima: valgono a prescindere dallo strumento |
| **Fatti di prodotto** [FATTO] | Funzionamento dei Project, memoria, policy dati | **Ogni 6 mesi** — e ogni volta che si nota un cambiamento |
| **Ricette e casi** | I template della Parte III, gli esempi | Continua: cambiano con i vostri processi |

## La revisione semestrale (un’ora, due volte l’anno)

1. Aprire i moduli con etichette [FATTO] (soprattutto F4 e F6) e riverificare ogni affermazione sulle fonti ufficiali (support.claude.com, privacy.claude.com).
2. Correggere ciò che è cambiato, aggiornare la data di verifica, incrementare la versione del modulo, scrivere il changelog.
3. Rileggere le ricette più usate: sono ancora il modo in cui lavoriamo?
4. Archiviare ciò che nessuno usa da un anno.

## Le regole di versionamento (già in uso in questo manuale)

- Ogni modulo ha **codice, versione, data, dipendenze, changelog**.
- Correzione minore → v0.1 → v0.2. Riscrittura → v1.0. Il changelog dice *cosa* è cambiato, non «aggiornamenti vari».
- Un modulo che cita un fatto di prodotto **dichiara la fonte e la data di verifica**: senza data, un [FATTO] non vale nulla.
- I sorgenti (.md) sono la fonte unica; sito e slide si rigenerano da lì.

## Chi è responsabile
Un **curatore del manuale** (ruolo, non persona a vita): raccoglie le segnalazioni, esegue la revisione semestrale, decide cosa entra. Le segnalazioni arrivano da chiunque, con una riga: «M9 R3 non funziona più così, ora Fattureincloud fa X».

> **LA REGOLA FINALE** Questo manuale non è un documento: è un processo. Se in sei mesi nessuno lo ha modificato, non significa che era perfetto — significa che nessuno lo sta usando.

*Changelog: v0.1 — prima stesura.*
