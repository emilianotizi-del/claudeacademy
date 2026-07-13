# A2 — I modelli mentali del software (per chi non lo scrive)

**Codice:** A2 · **Versione:** 0.1 · 13/07/2026 · Parte A · i concetti che servono per decidere, non per programmare
Non serve il vocabolario: serve capire **perché il software si comporta come si comporta**. Cinque modelli spiegano il 90% delle decisioni.

## 1 · Stato
Tutto il software è gestione dello stato: cosa il sistema ricorda tra un’azione e l’altra. La maggior parte dei bug gravi sono bug di stato: due copie della verità che divergono, un’azione eseguita due volte, un dato aggiornato mentre qualcun altro lo leggeva. **Domanda da committente:** dove vive la verità su questo dato, e chi ha il diritto di cambiarla?

## 2 · Confini
Un sistema è fatto di parti che si parlano attraverso confini (interfacce, API, contratti). La qualità di un’architettura si misura sulla qualità dei confini: quelli buoni permettono di sostituire una parte senza toccare le altre. **Domanda:** se domani volessi cambiare questo pezzo, quanto del resto dovrei riscrivere? Se la risposta è «molto», il confine è nel posto sbagliato.

## 3 · Accoppiamento e coesione
Accoppiamento: quanto una parte dipende dalle altre. Coesione: quanto ciò che sta insieme ha davvero a che fare. La regola è antica e non è invecchiata: **basso accoppiamento, alta coesione.** È il criterio con cui giudicare qualunque proposta di struttura, anche senza leggere il codice.

## 4 · Idempotenza e fallimento
Le cose falliscono a metà: la rete cade dopo che il pagamento è partito ma prima che tu lo registri. Un’operazione è **idempotente** se eseguirla due volte ha lo stesso effetto di eseguirla una volta. Nei sistemi distribuiti è la differenza tra un prodotto affidabile e uno che sanguina. **Domanda:** cosa succede se questa operazione viene ripetuta? E se si interrompe a metà?

## 5 · Costo del cambiamento
Il software non ha costo di produzione: ha costo di **modifica**. Ogni scelta di oggi è un prezzo che pagherai in flessibilità domani. Le architetture non si giudicano su «funziona», ma su **quanto costa cambiare idea**. Questo è il vero metro dell’architetto.

| Sento dire… | Traduco… | Chiedo… |
|---|---|---|
| «Lo mettiamo in un microservizio» | Aggiungiamo un confine di rete | Che problema risolve, e che complessità aggiunge? |
| «Refactoring» | Cambiare la struttura senza cambiare il comportamento | Cosa migliora concretamente? Come sappiamo che il comportamento non è cambiato? |
| «Scala» | Regge più carico | Quanto carico, davvero? Ne abbiamo evidenza o è immaginazione? |
| «Temporaneo» | Permanente | Cosa deve accadere perché venga rimosso, e chi lo farà? |

*Changelog: v0.1 — prima stesura.*
