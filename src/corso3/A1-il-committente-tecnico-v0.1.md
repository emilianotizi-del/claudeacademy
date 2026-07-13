# A1 — Il committente tecnico: la casella che l’AI ha aperto

**Codice:** A1 · **Versione:** 0.1 · 13/07/2026 · Parte A · teoria del ruolo
In un progetto software esistono tre caselle: **committente** (sa perché il sistema esiste, decide priorità e «quando è fatto»), **architetto** (decide come è fatto: struttura, dati, confini, compromessi), **esecutore** (scrive il codice). Storicamente la casella dell’architetto era inaccessibile a chi non programmava: le decisioni strutturali finivano di fatto all’esecutore, e il committente riceveva ciò che gli veniva dato. È questa geometria che l’AI cambia.

## Le tre asimmetrie che governano il rapporto con un esecutore-AI

1. **Asimmetria di velocità.** L’esecutore produce più in fretta di quanto tu possa verificare. È la differenza con un team umano, dove il collo di bottiglia era la produzione. Ora il collo di bottiglia sei tu — e la tentazione è smettere di verificare.
2. **Asimmetria di memoria.** L’esecutore non ricorda perché tre settimane fa avete scelto A invece di B. Le decisioni architetturali, se non sono scritte da qualche parte che il modello legge, non esistono: verranno silenziosamente contraddette.
3. **Asimmetria di compiacenza.** L’esecutore non ha interesse a dirti che stai chiedendo una sciocchezza. Un buon senior lo farebbe. Devi progettare tu i meccanismi di dissenso — non aspettartelo.

## Cosa resta irriducibilmente umano
Non la scrittura del codice. Restano: la **definizione del problema** (nessun modello sa quale problema vale la pena risolvere), i **compromessi con conseguenze fuori dal sistema** (costi, tempi, rischio legale, rapporti con i clienti), l’**accettazione** (dichiarare che una cosa è finita è un atto di responsabilità, non una misura tecnica), e la **teoria del sistema** — il modello mentale del perché le cose stanno come stanno. Peter Naur sosteneva che programmare è costruire una teoria, e che il codice ne è solo il residuo: se la teoria muore, il codice diventa inmanutenibile anche se compila. Con un esecutore-AI la teoria deve vivere **in te**, e in ciò che scrivi — perché il modello, tra una sessione e l’altra, la perde.

> **LA TESI DEL CORSO** La competenza scarsa non è più saper scrivere codice: è saper decidere, specificare e verificare. Il resto è diventato abbondante — e ciò che è abbondante non è più il vincolo.

*Changelog: v0.1 — prima stesura.*
