# A8 — Verificare codice che non hai scritto

**Corso 3 · Parte C · v1.0** — *il modulo centrale del corso*

Il problema fondamentale della committenza assistita da AI, in una riga:

> **Il volume prodotto supera la tua capacità di leggerlo.**

Non puoi verificare tutto. Quindi la soluzione **non è verificare di più**: è costruire un sistema in cui **le cose importanti non possano passare inosservate**. Verifica automatizzata dove il volume è alto, attenzione umana dove il rischio è alto.

---

## 1. La tassonomia dei bug che l'AI produce

Un modello non sbaglia come uno sviluppatore junior. Sapere *come* sbaglia è metà del lavoro di verifica.

### 1.1 Il codice plausibile ma falso
Chiama una funzione che non esiste, o che esiste con un'altra firma. **Innocuo**, perché non compila: il ciclo di feedback lo cattura subito. Non è questa la categoria pericolosa.

### 1.2 Il caso felice perfetto
Il codice funziona esattamente per lo scenario che hai descritto, e **crolla su tutto il resto**: input vuoto, valore nullo, doppia esecuzione, lista con un elemento, lista con centomila. È la categoria più frequente, perché il modello ottimizza per l'esempio che gli hai dato.

### 1.3 La reinvenzione silenziosa
Riscrive una funzione che nel tuo sistema esiste già, leggermente diversa. Ora hai **due verità** che divergeranno (→ A2). Nessun test lo rileva, perché entrambe funzionano — finché non si contraddicono.

### 1.4 La violazione di un vincolo che non era nel contesto
Tocca un modulo che non doveva toccare, cambia una convenzione, contraddice una decisione presa un mese fa. **Sintomo dell'asimmetria di memoria** (→ A1): non è cattiveria, è assenza.

### 1.5 La sicurezza per omissione
Non aggiunge una vulnerabilità: **dimentica un controllo**. La query non filtra per l'utente corrente, l'endpoint non verifica i permessi, l'input non è validato. **È la categoria più pericolosa in assoluto**, perché il codice sembra corretto, funziona in tutte le prove, e la falla si vede solo se la cerchi apposta (→ A9).

### 1.6 La correttezza locale con danno globale
Ogni pezzo è giusto; insieme non funzionano. Due funzioni che scrivono lo stesso dato con assunzioni diverse; una modifica che rispetta il contratto ma rompe un'aspettativa non scritta.

### 1.7 Il codice orfano
Genera struttura, astrazioni, livelli — che nessuno userà mai. Non è un bug: è **peso morto** che confonde il prossimo che passa (che sarai tu, tra tre mesi, o il modello stesso, che lo prenderà per una convenzione da imitare).

**La regolarità da tenere a mente:** l'AI sbaglia dove **non hai specificato**, non dove hai specificato male. Gli errori si annidano nei silenzi.

---

## 2. Le cinque difese, in ordine di potenza

### 2.1 Il ciclo di feedback automatico (la più potente)
Compilatore, type-checker, linter, test. Sono **verificatori che non si stancano e non si fidano.**

L'insight che cambia tutto: **il valore di questi strumenti si moltiplica in presenza di un agente**, perché l'agente li usa da solo, sbaglia, se ne accorge, corregge — senza consumare il tuo tempo. Un progetto senza ciclo di feedback obbliga *te* a essere il compilatore. È il lavoro peggiore del mondo, e lo farai male.

**Priorità di allestimento, prima di ogni funzionalità:**
1. Linguaggio con tipi statici (il type-checker cattura gratis un'intera classe di errori).
2. Comando unico: `check` = tipi + lint + test.
3. Il comando gira **automaticamente** prima di ogni commit e in CI.

### 2.2 I test — non per dimostrare, ma per affermare
**I test non dimostrano che il codice è corretto.** Servono a **dire ad alta voce cosa deve essere vero**, e ad accorgersi quando smette di esserlo.

Come committente non li scrivi: **li commissioni e li leggi**. E la domanda che devi saper fare non è "ci sono i test?" ma:

> **"Che cosa affermano, esattamente?"**

Un test che verifica che `somma(2,2) == 4` non protegge nulla. Un test che verifica che *un socio con quota non pagata non compare tra gli aventi diritto al voto* protegge il prodotto.

**Cosa merita un test, in ordine:**
1. **Le regole di dominio** — quelle che, se violate, causano un danno reale. Sono poche, sono tue, e nessuno le indovina al posto tuo.
2. **I bug già accaduti** — ogni bug corretto diventa un test, sempre. È l'unico modo per non ripagare mai due volte lo stesso prezzo.
3. **I confini** — cosa succede con input vuoti, nulli, enormi, malformati, doppi.
4. **I percorsi che toccano soldi, permessi, dati personali.**

**Cosa NON merita un test:** il codice banale, l'interfaccia che cambia ogni settimana, i dettagli implementativi (un test che si rompe a ogni refactoring innocuo è un test che verrà cancellato — e giustamente).

**Il prompt:**
```
Prima di implementare, scrivi i test per questi criteri di accettazione.
Poi elencami in italiano cosa afferma ciascun test, una riga per test.
Voglio verificare che stiate proteggendo le cose giuste, non che i test
esistano.
```

### 2.3 La revisione avversariale (la più sottovalutata)
**Regola non negoziabile: il revisore non può essere la conversazione che ha prodotto il codice.** Non è una questione di lealtà: è un fatto osservabile. Chi ha appena costruito qualcosa, umano o modello, ha nel contesto tutte le ragioni per cui l'ha fatto — e nessuna delle ragioni per cui è sbagliato.

**Sessione nuova, contesto minimo, mandato ostile:**

```
Ecco un codice scritto da qualcun altro. Il tuo compito è trovare i modi in
cui si rompe. Non commentare lo stile, non dire cosa è fatto bene.

1. Quali input lo fanno fallire? Elenca i casi concreti.
2. Cosa succede se viene eseguito due volte in parallelo?
3. Quali assunzioni implicite fa sul resto del sistema? Quali di queste
   potrebbero essere false?
4. Un utente malintenzionato cosa potrebbe ottenere? Prova a bypassare i
   controlli.
5. Quali controlli di sicurezza o autorizzazione MANCANO (non quelli
   presenti e sbagliati: quelli assenti)?
6. Cosa non è stato gestito perché nessuno ci ha pensato?
```

Il punto 5 va cerchiato in rosso: **è il modo di trovare la categoria 1.5**, quella che ti farebbe più male.

### 2.4 La lettura del diff (l'unica sostenibile)
**Non leggere il sistema: leggi il delta.** È l'unica pratica di lettura che regge nel tempo.

Cosa cerchi, in cinque minuti:
- **File toccati che non c'entravano** con il compito → segnale d'allarme immediato.
- **Cancellazioni**: cosa è sparito? Le rimozioni sono più pericolose delle aggiunte, e passano inosservate.
- **Dipendenze nuove**: da dove arriva quella libreria? Chi la mantiene?
- **Numeri e stringhe magiche**, valori scritti a mano, credenziali.
- **Gestione degli errori**: c'è, o gli errori vengono silenziosamente ingoiati? *Un `catch` vuoto è un bug che si presenterà tra sei mesi, senza tracce.*
- **Query al database**: filtrano per utente/cliente? (→ A9)

### 2.5 Le invarianti nel sistema
Le regole che devono essere **sempre** vere, scritte dove non possono essere aggirate: vincoli nel database (→ A7), controlli all'ingresso dei moduli, allarmi che scattano se una condizione impossibile si verifica.

> **Un test verifica ciò a cui hai pensato. Un'invariante blocca anche ciò a cui non hai pensato.**

---

## 3. Le tre illusioni

### «Funziona» ≠ «è corretto»
Funziona *sui casi che hai provato*. I bug che contano vivono negli altri. Il modello ha ottimizzato per il tuo esempio: è proprio bravo a farlo funzionare **lì**.

### «È leggibile» ≠ «fa ciò che sembra»
Il codice generato è spesso **più pulito della media**: nomi sensati, commenti, struttura ordinata. Questo **aumenta la fiducia senza aumentare la correttezza**, ed è il tranello più insidioso di tutti — perché la bruttezza, storicamente, era il segnale che ti faceva rallentare.

### «L'ho fatto rivedere all'AI» ≠ «è stato rivisto»
Se lo chiedi nella stessa conversazione, difenderà il proprio lavoro. La revisione ha valore **solo se il revisore è cieco al contesto del produttore**.

---

## 4. La verifica asimmetrica: dove spendere l'attenzione

Non tutto merita lo stesso rigore. **Alloca l'attenzione dove il danno è alto**, non dove il codice è complicato.

| Zona | Danno se sbaglia | Verifica |
|---|---|---|
| Interfaccia, stile, testi | Estetico | Guardo che funzioni. Basta. |
| Logica di dominio | Decisioni sbagliate, dati sporchi | Test sulle regole + lettura del diff |
| **Soldi** | Perdita economica, contenzioso | Test + revisione avversariale + **doppia verifica umana** |
| **Permessi e visibilità dei dati** | **Fuga di dati, danno legale** | Test + revisione avversariale mirata + **test di isolamento fra clienti** |
| **Migrazioni di dati** | **Perdita irreversibile** | Prova su copia + conteggi prima/dopo + piano di ritorno |
| Cancellazioni | Perdita irreversibile | Come sopra, e mai in automatico |

**La colonna centrale è la sola che conta.** Un committente maturo si riconosce da questo: non da quanto verifica, ma da **dove** verifica.

---

## 5. La checklist di accettazione

Prima di dichiarare finito un pezzo di lavoro:

```
□ I test passano — e ho letto COSA affermano, non solo che sono verdi
□ Ho letto il diff per intero
□ Nessun file toccato che non c'entrava
□ Gli errori sono gestiti, non ingoiati (nessun catch vuoto)
□ Le query filtrano per utente/cliente dove serve
□ Nessun segreto, nessun dato reale, nessun valore magico
□ È stato provato sui casi storti, non solo su quello felice
□ Una sessione avversariale ci ha guardato, e ho risposto ai suoi rilievi
□ Il Decision Log è aggiornato, se ci sono state scelte strutturali
□ So dire in due frasi cosa fa questo codice e perché è fatto così
```

**L'ultima riga è la più importante.** Se non sai dirlo, non lo possiedi — e ciò che non possiedi ti possiede: sarà lui a dettarti i tempi, il giorno in cui si romperà.

---

## Strumenti di questo modulo

1. **La tassonomia dei sette modi in cui l'AI sbaglia** — per sapere dove guardare.
2. **Il ciclo di feedback** come moltiplicatore di ogni altra difesa.
3. **Il prompt "cosa affermano i test"** — l'unico modo di giudicarli senza saperli scrivere.
4. **Il prompt di revisione avversariale**, con la domanda sui controlli *mancanti*.
5. **La lista di lettura del diff in cinque minuti.**
6. **La matrice di verifica asimmetrica** — dove spendere l'attenzione.
7. **La checklist di accettazione.**

---

## Errori tipici

1. **Fidarsi della bellezza del codice.**
2. **Chiedere la revisione a chi ha prodotto.**
3. **Test che verificano l'ovvio** e non le regole di dominio.
4. **Leggere il codice invece del diff** — insostenibile, quindi presto abbandonato del tutto.
5. **Verificare tutto allo stesso modo**: si esaurisce l'attenzione dove non serve e non ne resta dove serve.
6. **Non trasformare ogni bug in un test.** Chi non lo fa, paga due volte lo stesso prezzo — e la seconda volta di solito è peggio.

*v1.0 — riscrittura in profondità.*
