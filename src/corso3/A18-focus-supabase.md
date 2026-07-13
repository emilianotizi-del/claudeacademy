# A18 — Focus: Supabase, dal punto di vista dell'architetto

**Corso 3 · Parte F · Focus di stack · v1.0**
*Fonti verificate il 13/07/2026: supabase.com/docs (Row Level Security, Securing your API, Securing your data).*

Supabase è Postgres con un'API HTTP davanti. **Questa singola frase contiene tutto ciò che devi capire — e tutti i modi in cui ci si fa male.**

Perché "un'API davanti al database" significa che **il tuo database è esposto a Internet**, e l'unica cosa che separa i tuoi dati da chiunque è la configurazione che hai fatto (o dimenticato di fare). Non è un difetto: è il patto. Ma va conosciuto.

---

## 1. Il modello mentale corretto

```
Browser  ──[chiave publishable, pubblica]──►  PostgREST  ──►  Postgres
                                                   ▲
                                          RLS: l'unica difesa
Server   ──[chiave secret / service_role]──────────┴──► Postgres (BYPASSA TUTTO)
```

**Due chiavi, due mondi:**

| Chiave | Dove vive | Cosa fa |
|---|---|---|
| **publishable** (già *anon*) | Nel browser. **È pubblica per progetto.** Chiunque apre il tuo sito la vede | Può fare **solo** ciò che le RLS permettono |
| **secret / service_role** | **Solo sul server.** Mai, in nessuna circostanza, nel browser | **Bypassa le RLS.** È l'accesso root al database |

> **La chiave publishable non è un segreto e non deve esserlo. La sicurezza non sta nella chiave: sta nelle policy.** Chi ragiona come se la chiave fosse una password ha già perso, perché quella chiave è nel bundle JavaScript di ogni visitatore.

---

## 2. RLS: la cosa che devi capire davvero

Row Level Security è una funzione di Postgres: **aggiunge implicitamente una condizione a ogni query**. Con RLS attiva e una policy scritta bene, una query che dimentica il filtro **non restituisce le righe altrui: non ne restituisce nessuna.**

È esattamente la difesa strutturale di cui parlavo in A9 e A17: **l'autorizzazione messa dove non si può dimenticare.** Ed è il motivo per cui Supabase, usato bene, è più sicuro di un backend tradizionale scritto in fretta — dove il filtro dipende dal fatto che uno sviluppatore (o un agente) si ricordi di scriverlo in ognuna delle 200 query.

### Le tre regole non negoziabili

**1 · RLS attiva su OGNI tabella dello schema esposto (`public`). Nessuna eccezione.**

[FATTO, docs] La RLS viene attivata **automaticamente solo** se crei la tabella dal Table Editor della dashboard. **Se la crei via SQL, via migrazione, o tramite un agente AI — non viene attivata.**

Rileggi la frase precedente, perché è **la singola causa più frequente di fughe di dati nei progetti costruiti con l'AI**. L'agente scrive `CREATE TABLE`, la tabella è raggiungibile via API dal primo istante, l'app funziona benissimo in sviluppo (un utente), funziona in produzione (un utente), e **funziona anche per l'utente B che digita l'ID dell'utente A**.

La verifica, da eseguire come un rito, dopo ogni sessione che tocca lo schema:
```sql
SELECT tablename, rowsecurity
FROM pg_tables
WHERE schemaname = 'public' AND rowsecurity = false;
```
**Ogni riga in quel risultato è una tabella leggibile da chiunque abbia il tuo URL.** Il risultato deve essere vuoto. Sempre.

**2 · RLS attiva senza policy = tabella inaccessibile.** È il default sicuro: nega tutto. Quindi l'ordine giusto è **attiva RLS, poi scrivi le policy** — mai il contrario, perché nel frattempo la tabella è aperta.

**3 · La chiave `service_role` non tocca mai il browser.** [FATTO, docs] Bypassa le RLS: se finisce nel bundle, hai regalato il database. I percorsi di fuga tipici sono banali e frequentissimi: messa in una variabile `NEXT_PUBLIC_*` (che per definizione finisce nel browser), committata "solo per la demo", stampata nei log all'avvio, restituita in un messaggio d'errore.

---

## 3. Il Security Advisor: il tuo alleato più economico

[FATTO] Supabase include un **Security Advisor** nella dashboard: un linter che segnala esattamente i problemi di questo modulo — RLS disattivata su tabelle pubbliche, RLS attiva senza policy, viste `security definer`, funzioni con `search_path` mutabile, colonne sensibili esposte.

**Regola operativa: si esegue dopo ogni modifica allo schema.** È gratis, dura dieci secondi, e cattura la classe di errori che l'AI produce più spesso. È l'equivalente del "comando unico di verifica" (→ A4), per la parte database.

---

## 4. Il multi-tenancy in Supabase (il pattern che devi conoscere)

Riprendo A17 §3 e lo rendo concreto.

### La policy ingenua che ti tradirà
```sql
create policy "membri leggono la propria organizzazione"
on documenti for select
using (
  organizzazione_id in (
    select organizzazione_id from membri where utente_id = auth.uid()
  )
);
```
Funziona. **Ma:** questa sotto-query viene valutata **per ogni riga esaminata**. Su tabelle grandi, la policy che ti protegge diventa la ragione per cui il sistema è inutilizzabile — e a quel punto la tentazione (sbagliatissima) è disattivarla.

### Il pattern corretto
[BP, docs] Due mosse, entrambe documentate ufficialmente:

**a) Avvolgi le funzioni in un `select`**, così il risultato viene calcolato una volta e messo in cache dal planner:
```sql
using ( organizzazione_id in (select ...) )   -- non ripetuto per riga
```
E lo stesso vale per `auth.uid()`: si scrive `(select auth.uid())`, non `auth.uid()` nudo.

**b) Usa una funzione `security definer`** per la ricerca delle appartenenze, così quella lettura non paga a sua volta il costo delle RLS:
```sql
create function ha_ruolo_su(org uuid) returns boolean
language sql security definer stable
set search_path = ''            -- ← obbligatorio: search_path mutabile = vulnerabilità
as $$
  select exists (
    select 1 from public.membri
    where utente_id = (select auth.uid()) and organizzazione_id = org
  );
$$;
```
[FATTO, docs] **Le funzioni `security definer` non devono mai vivere in uno schema esposto** dall'API. E `set search_path = ''` non è un vezzo: senza, la funzione è aggredibile.

**c) Metti sempre `to authenticated`** nelle policy: senza, vengono valutate anche per il ruolo anonimo, inutilmente.

**d) Indicizza la colonna del tenant.** La policy filtra su `organizzazione_id`: senza indice, ogni query è una scansione completa (→ A11).

### La regola di prestazione che sintetizza tutto
> **Filtra esplicitamente nella query anche ciò che la RLS filtrerebbe comunque.** `.eq('organizzazione_id', org)` nel codice, *oltre* alla policy. La RLS è la **difesa**; il filtro esplicito è la **prestazione** — e permette a Postgres di usare gli indici. Averli entrambi non è ridondanza: è cintura più bretelle, dove la cintura protegge e le bretelle fanno andare veloce.

---

## 5. Il test che scrivi una volta e ti salva per sempre

Da A9 e A17, calato su Supabase:

```
Scrivi un test di isolamento fra tenant:
1. Crea due organizzazioni, A e B, ciascuna con un utente e alcuni record.
2. Autenticati come utente di A (con la chiave publishable, NON con
   service_role).
3. Per OGNI tabella dello schema public, tenta:
   - una select di tutte le righe → deve restituire solo quelle di A
   - una select per ID diretto di un record di B → deve restituire 0 righe
   - un insert con organizzazione_id di B → deve fallire
   - un update e un delete su un record di B → devono fallire
4. Il test fallisce se anche UNA sola di queste operazioni riesce.
```

**Questo test è la cosa più importante che scriverai nel progetto.** Va eseguito in CI, a ogni modifica dello schema, per sempre. Ed è il motivo per cui potrai dormire mentre un agente genera migrazioni.

---

## 6. Le altre cinque insidie

**Storage.** [FATTO, docs] I bucket sono privati per default, e le policy sono le stesse RLS applicate a `storage.objects`. **Un bucket "pubblico" serve qualunque file a chiunque abbia l'URL** — e i percorsi dei file spesso contengono l'ID del tenant, il che rende gli URL indovinabili. Pubblico solo per ciò che è davvero destinato al web aperto.

**Viste.** [FATTO, docs] Le viste, per default in Postgres, **bypassano le RLS** delle tabelle sottostanti (sono create come `security definer`). Da Postgres 15 si può imporre il rispetto delle policy con `security_invoker = true`. **Una vista dimenticata è una porta sul retro.**

**Migrazioni generate da agenti.** Ogni `CREATE TABLE` generato è una tabella potenzialmente senza RLS. **Regola per il file di istruzioni del progetto** (→ A4):
```
Ogni migrazione che crea una tabella in `public` DEVE, nello stesso file:
1. abilitare RLS: alter table X enable row level security;
2. definire le policy per select/insert/update/delete, con `to authenticated`;
3. creare l'indice sulla colonna del tenant.
Una migrazione che crea una tabella senza questi tre elementi è incompleta:
non proporla.
```
Questa è la regola più preziosa che scriverai in `CLAUDE.md`.

**Edge Functions.** Girano sul server e hanno accesso ai segreti, `service_role` incluso. **Trattale come servizi privilegiati:** non stampare l'ambiente nei log, non restituire dettagli interni negli errori, e verifica sempre il JWT del chiamante.

**`update` senza `select`.** [FATTO, docs] Una policy di UPDATE **richiede anche una policy di SELECT** per funzionare: Postgres deve leggere la riga prima di modificarla. È un inciampo classico che fa sembrare rotto ciò che è solo incompleto.

---

## 7. Cosa Supabase ti dà, e che dovresti usare

- **Postgres vero.** Il che significa (→ A6): **i tuoi dati sono portabili.** È la ragione strategica più forte per stare su Supabase invece che su un database proprietario: se un giorno te ne vai, i dati sono uno standard, non un ostaggio.
- **Vincoli e transazioni** (→ A7): usali. Chiavi esterne, unicità, `check`. Ogni vincolo è una classe di bug che l'agente non potrà introdurre.
- **Migrazioni versionate**: nel repo, in ordine, applicate in CI. Mai modifiche a mano dalla dashboard in produzione (→ A5).
- **`EXPLAIN ANALYZE`**: incolla l'output a un modello quando una query è lenta. Lì i dati ci sono, e la diagnosi è affidabile (→ A11).

---

## Checklist Supabase

```
□ Security Advisor eseguito, zero segnalazioni
□ Nessuna tabella in `public` con rowsecurity = false
□ Ogni tabella ha policy per select/insert/update/delete, con `to authenticated`
□ Le funzioni di supporto sono security definer, fuori dallo schema esposto,
  con search_path = ''
□ service_role NON compare in nessuna variabile NEXT_PUBLIC_*, nel bundle,
  nei log, nel repository
□ Test di isolamento fra tenant in CI, verde
□ Indice sulla colonna del tenant
□ auth.uid() avvolto in (select ...) nelle policy
□ Bucket privati salvo eccezione motivata
□ Viste: security_invoker = true, oppure verificate una a una
□ Le migrazioni vivono nel repo, non nella dashboard
```

---

## Errori tipici

1. **Creare tabelle via SQL o via agente e dimenticare RLS.** La causa numero uno, e vale la pena ripeterla tre volte.
2. **Credere che la chiave publishable sia un segreto.** Non lo è, e non deve esserlo.
3. **`service_role` nel client** — spesso attraverso una variabile `NEXT_PUBLIC_*` messa lì da un agente per far funzionare qualcosa in fretta.
4. **Policy con sotto-query non avvolte**, che rendono il sistema lento e la sicurezza un capro espiatorio.
5. **Fidarsi delle RLS per le prestazioni:** filtra anche esplicitamente.
6. **Viste e bucket dimenticati.** Le porte sul retro sono sempre quelle che non guardi.

*v1.0 — focus di stack; fatti verificati su supabase.com/docs il 13/07/2026.*
