# A3 — Specificare: l’arte di dire cosa vuoi in modo eseguibile

**Codice:** A3 · **Versione:** 0.1 · 13/07/2026 · Parte A · dalla specifica ai criteri di accettazione
Con un esecutore infinitamente veloce, **la specifica è il collo di bottiglia e il punto di leva**. Una specifica ambigua non produce un ritardo: produce un sistema sbagliato costruito velocemente.

## L’anatomia di una specifica che funziona

1. **Contesto e problema** — perché esiste. Senza, ogni compromesso verrà deciso a caso.
2. **Casi d’uso** — chi fa cosa, in che ordine, con quali eccezioni. Le eccezioni sono il vero contenuto: il caso felice lo indovina chiunque.
3. **Modello dei dati** — le entità e le loro relazioni. È la parte che invecchia peggio se sbagliata (→ A8).
4. **Criteri di accettazione** — «è fatto quando…», osservabili, non opinabili. Se non sai dire quando è finito, non hai specificato: hai desiderato.
5. **Fuori scopo esplicito** — ciò che il sistema NON farà. Il pezzo che tutti saltano e che decide se il progetto convergererà mai.
6. **Vincoli non funzionali** — prestazioni, sicurezza, privacy, costi, tempi. Sono requisiti, non aspirazioni.

## Il paradosso della specifica
Una specifica completa e non ambigua **è** un programma: se la scrivi al massimo dettaglio, hai programmato in prosa. Il mestiere sta nel trovare il livello giusto: **abbastanza preciso da vincolare le decisioni che contano, abbastanza aperto da lasciare all’esecutore quelle che non contano.** Vincolare troppo poco produce sistemi arbitrari; vincolare troppo produce lentezza e specifiche che nessuno mantiene.

## La tecnica del «pre-mortem»
Prima di approvare una specifica: *«siamo tra sei mesi, il progetto è fallito. Racconta perché.»* Fatto seriamente, questo esercizio scopre più rischi di qualunque analisi ottimistica — perché rimuove la resistenza psicologica a immaginare il fallimento.

**Prompt · revisione avversariale di una specifica**
```
Ecco la specifica di [SISTEMA]. Non implementarla. Prima:
1) elenca ogni ambiguità che ti costringerebbe a indovinare;
2) elenca i casi limite non coperti (dati mancanti, azioni concorrenti, fallimenti a metà, utenti malintenzionati);
3) dimmi quali decisioni architetturali questa specifica mi sta facendo prendere implicitamente, senza che io me ne accorga;
4) scrivi i criteri di accettazione che mancano.
```

*Changelog: v0.1 — prima stesura.*
