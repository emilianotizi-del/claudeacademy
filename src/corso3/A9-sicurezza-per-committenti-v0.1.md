# A9 — Sicurezza: il modello di minaccia per committenti

**Codice:** A9 · **Versione:** 0.1 · 13/07/2026 · Parte C · pensare come chi vuole entrare
La sicurezza non è una funzionalità che si aggiunge: è una **proprietà del sistema**, e come tutte le proprietà va progettata all’inizio. Il committente non deve saperla implementare: deve saper **fare le domande giuste e riconoscere le risposte evasive**.

## Il modello di minaccia in quattro domande

1. **Cosa proteggo?** (i dati dei soci, l’integrità di un voto, la disponibilità del servizio, la reputazione)
2. **Da chi?** (un curioso, un socio scontento, un concorrente, un attaccante automatico, un dipendente infedele — sono avversari con capacità e motivazioni diversissime)
3. **Quali sono le vie d’ingresso?** (autenticazione, input degli utenti, dipendenze di terze parti, l’amministratore stesso, il fornitore cloud)
4. **Cosa succede quando — non se — accade?** Rilevazione, contenimento, notifica agli interessati, ripristino. Un sistema che non può accorgersi di essere stato violato è già violato.

## I principi che non cambiano

- **Privilegio minimo:** ciascuno accede al minimo necessario. Vale per gli utenti, per i servizi e per te.
- **Difesa in profondità:** nessun controllo singolo deve essere l’unico ostacolo.
- **Non fidarsi dell’input:** ogni dato che arriva da fuori è ostile finché non è validato — anche quello che arriva dal tuo stesso frontend.
- **Non inventare crittografia né autenticazione:** è l’unico dominio in cui «l’ho fatto io» è quasi sempre la risposta sbagliata.
- **Sicurezza per default:** l’impostazione predefinita è chiusa; l’apertura è una decisione esplicita.

## Il dato personale come passivo
Contabilmente parlando, i dati personali non sono un asset: sono un **passivo**. Ogni dato raccolto è un obbligo (proteggerlo, giustificarlo, cancellarlo su richiesta) e un rischio (se esce, il danno è tuo). La minimizzazione non è un adempimento burocratico: è riduzione dell’esposizione. **Il dato più sicuro è quello che non hai raccolto.**

*Changelog: v0.1 — prima stesura.*
