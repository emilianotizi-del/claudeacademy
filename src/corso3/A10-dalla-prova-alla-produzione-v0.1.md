# A10 — Dalla prova alla produzione

**Codice:** A10 · **Versione:** 0.1 · 13/07/2026 · Parte C · ciò che cambia quando qualcuno dipende da te
La produzione non è uno stadio di maturità del codice: è uno **stato di obbligo**. Dal momento in cui una persona reale ci lavora, hai contratto un debito verso di lei — e i tuoi errori diventano suoi problemi.

## Cosa cambia, concretamente

|  | Prototipo | Produzione |
|---|---|---|
| I dati | Si possono buttare | Sono la verità di qualcuno |
| Il downtime | Irrilevante | È un danno, e va comunicato |
| Il rilascio | Quando ti va | Con procedura, e reversibile |
| I bug | Si scoprono usando | Si scoprono dai clienti — se non hai osservabilità |
| Il codice brutto | Un problema tuo | Un debito che paghi ogni mese |

## La migrazione dei dati reali: il momento più pericoloso
L’importazione dei dati esistenti (l’Excel dei soci) è dove muoiono i progetti seri. Regole: **prova su copia**, **conta prima e dopo** (quanti record entrano, quanti escono, perché la differenza), **conserva l’originale immutato**, **prevedi il ritorno indietro**, e **fai validare il risultato a chi conosce i dati** — non a chi ha scritto la migrazione. I dati sporchi non sono un’eccezione: sono la norma.

## Il rilascio come atto reversibile
Rilasci piccoli e frequenti sono più sicuri di rilasci grandi e rari — controintuitivo, ma è tra i risultati più solidi dell’ingegneria del software degli ultimi vent’anni: **la dimensione del cambiamento predice il rischio meglio di qualunque altra variabile.** Il grande rilascio «attentamente pianificato» è quello che ti tiene sveglio la notte.

*Changelog: v0.1 — prima stesura.*
