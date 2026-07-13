# A4 — Dirigere un esecutore-AI: context engineering

**Codice:** A4 · **Versione:** 0.1 · 13/07/2026 · Parte B · come si struttura il lavoro di un agente di codice
Un agente di codice non è una chat che restituisce testo: legge il repository, modifica file, esegue comandi, legge gli errori e riprova. Cambia il modo in cui va diretto.

## I quattro artefatti che rendono governabile un agente

1. **Il file di istruzioni permanente del progetto.** La costituzione: convenzioni, struttura, comandi, ciò che non va mai fatto. È l’equivalente delle istruzioni di Project (→ F4). Senza, ogni sessione riparte da presupposti diversi.
2. **Il registro delle decisioni** (una riga per decisione: cosa, perché, alternative scartate). Combatte l’asimmetria di memoria (→ A1): senza, l’agente contraddirà silenziosamente scelte che avevi già fatto.
3. **Il piano prima dell’azione.** Farsi produrre il piano, discuterlo, approvarlo, poi far eseguire. Correggere un piano costa minuti; correggere un’implementazione sbagliata costa ore — e a volte non ci si accorge nemmeno.
4. **Il ciclo di feedback automatico** (test, controlli, esecuzione). Un agente che può verificare da solo se ha rotto qualcosa è di un altro ordine di utilità rispetto a uno che non può.

## La gestione del contesto è il mestiere
La qualità del lavoro di un agente dipende quasi interamente da **cosa ha davanti**. Troppo poco: inventa. Troppo: si perde e le istruzioni importanti vengono annegate. Il lavoro dell’architetto è **curare il contesto**: dare i file giusti, il piano, i vincoli, e nient’altro. È la versione avanzata di F4.

## Sessioni corte, compiti chiusi
La sessione lunga degrada: si accumulano tentativi, errori corretti, direzioni abbandonate. La disciplina è quella di F3 portata all’estremo: **un compito, un risultato verificato, si consolida, si chiude.** Le sessioni-fiume producono sistemi che nessuno capisce, incluso chi li ha commissionati.

*Changelog: v0.1 — prima stesura.*
