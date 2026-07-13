# A7 — Progettare i dati: la decisione che si paga per anni

**Codice:** A7 · **Versione:** 0.1 · 13/07/2026 · Parte C · il modello dati come vincolo strutturale
**Il codice si riscrive in un pomeriggio; i dati no.** Il codice è diventato economico; il modello dei dati è rimasto costoso, perché una volta che ci sono dentro dati veri di clienti veri, cambiarlo significa migrazioni, incoerenze, downtime. È la decisione architetturale a più lunga vita — e quella su cui va speso il tempo che l’AI ti ha liberato.

## Le domande che rivelano un modello sbagliato

1. **Qual è la fonte di verità di ogni dato?** Se un’informazione esiste in due posti, prima o poi divergeranno. Sempre.
2. **Cosa cambia nel tempo, e serve la storia?** Uno stato che sovrascrive il precedente cancella il passato. In un sistema di registro (chi era socio quando si è votato?) la storia **è** il prodotto: si registrano eventi, non solo stati finali.
3. **Cosa è vero per definizione e cosa è calcolato?** Ciò che può essere derivato non va memorizzato: memorizzarlo crea una seconda verità che divergerà.
4. **Le identità sono stabili?** Un identificativo che cambia (l’email come chiave) è una bomba a orologeria.
5. **Cosa succede alle cancellazioni?** Cancellare davvero o marcare come cancellato cambia tutto — e il diritto all’oblio rende la domanda giuridica, non solo tecnica.

## Normalizzare, denormalizzare, e chi lo decide
La normalizzazione (una verità, un posto) è il punto di partenza corretto. La denormalizzazione (duplicare per andare più veloce) è un’ottimizzazione che si paga in coerenza. **Chi la propone deve portare un’evidenza di lentezza misurata, non un timore.** Ottimizzare prima di misurare è il modo più diffuso di rendere un sistema fragile senza renderlo veloce.

*Changelog: v0.1 — prima stesura.*
