# A8 — Verificare codice che non hai scritto

**Codice:** A8 · **Versione:** 0.1 · 13/07/2026 · Parte C · il modulo centrale del corso
È il problema fondamentale della committenza tecnica assistita da AI: **il volume di codice prodotto supera la tua capacità di leggerlo.** Non puoi verificare tutto. Quindi non devi verificare *tutto*: devi costruire un sistema in cui **le cose importanti non possono passare inosservate**.

## I cinque strumenti di verifica, in ordine di potenza

1. **Test automatici.** Non servono a dimostrare che il codice è corretto: servono a **dire ad alta voce cosa deve essere vero**, e ad accorgersi quando smette di esserlo. Come committente non devi scriverli: devi pretenderli, e devi saper leggere *cosa* affermano. Un test che verifica che 2+2=4 non protegge nulla; un test che verifica che un socio moroso non risulta in regola protegge il prodotto.
2. **Revisione avversariale.** Una sessione pulita, senza il contesto di chi ha scritto: «ecco il codice, trova i modi in cui si rompe; ipotizza un utente malintenzionato; cerca le assunzioni implicite». Il valore sta nell’assenza di contesto: chi ha scritto è innamorato della propria soluzione, anche se è una macchina.
3. **Lettura del diff, non del codice.** Non leggere il sistema: leggi **cosa è cambiato**. È sostenibile, e coglie il 90% delle sorprese.
4. **Invarianti e controlli automatici.** Le regole che devono essere sempre vere («nessuna quota registrata senza socio esistente») messe nel sistema, non nella buona volontà.
5. **Osservabilità.** Log, tracciamento, allarmi. La verifica non finisce al rilascio: in produzione un sistema fa cose che nessun test aveva previsto.

## Le tre illusioni

- **«Funziona» ≠ «è corretto».** Funziona sui casi che hai provato. I bug che contano vivono nei casi che non hai provato.
- **«Il codice è leggibile» ≠ «fa ciò che sembra».** Il codice generato è spesso più leggibile della media — e questo *aumenta* la fiducia ingiustificata.
- **«L’ho fatto rivedere all’AI» ≠ «è stato rivisto».** Se glielo chiedi nella stessa conversazione, ti confermerà il proprio lavoro (→ F1). La revisione ha valore solo se il revisore è cieco al contesto del produttore.

> **LA REGOLA** Non chiedere «funziona?». Chiedi «come faccio a sapere che funziona anche quando non sto guardando?».

*Changelog: v0.1 — prima stesura.*
