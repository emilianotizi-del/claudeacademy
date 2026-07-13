# A5 — La rete di sicurezza: versionamento, ambienti, reversibilità

**Codice:** A5 · **Versione:** 0.1 · 13/07/2026 · Parte B · perché poter tornare indietro cambia ciò che si può osare
Il principio non è tecnico ma strategico: **la reversibilità determina la velocità sostenibile.** Se ogni azione è irreversibile, ogni azione richiede cautela e tutto rallenta. Se puoi tornare indietro in trenta secondi, puoi permetterti di sbagliare in fretta — che è il modo più rapido di imparare.

## Le tre reti

| Rete | Cosa protegge | La domanda che ti salva |
|---|---|---|
| **Versionamento** (git) | Il codice: ogni stato funzionante è recuperabile | Se questa modifica è un disastro, come torno indietro e quanto ci metto? |
| **Ambienti separati** | I dati veri: si sperimenta dove non si fa male a nessuno | Sto lavorando su dati veri? Perché? |
| **Backup e ripristino provato** | L’esistenza dell’azienda | Quando è stata l’ultima volta che abbiamo **provato** a ripristinare? Un backup mai ripristinato non è un backup: è una speranza. |

## I segreti
Chiavi, token e password non stanno nel codice, non stanno nelle chat, non stanno nei messaggi. Stanno in un gestore di segreti, con **rotazione periodica** e revoca immediata quando escono dal loro perimetro. Un segreto che ha viaggiato in un canale non sicuro è **bruciato**: si revoca, non si spera. (Nota: il token usato in questo percorso è esattamente questo caso — è transitato in chat, e andrebbe ruotato.)

> **IL PRINCIPIO** Progetta perché sia facile disfare, non perché sia impossibile sbagliare. L’impossibilità di sbagliare non esiste; la facilità di rimediare sì.

*Changelog: v0.1 — prima stesura.*
