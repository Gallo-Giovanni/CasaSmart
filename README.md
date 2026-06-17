# CasaSmart

CasaSmart è un ecosistema di web app pensate per semplificare la gestione della spesa e della casa. Ogni app è un file HTML autonomo e condivide lo stesso progetto Supabase: autenticazione unica, database unico, account condiviso tra le due app.

---

## Indice

- [App disponibili](#app-disponibili)
- [SpesaSmart](#spesasmart)
  - [Funzionalità principali](#funzionalità-principali)
  - [Flusso di utilizzo](#flusso-di-utilizzo)
  - [Uso dell'AI](#uso-dellai)
- [SalvaSpreco](#salvaspreco)
  - [Funzionalità principali](#funzionalità-principali-1)
- [Stack tecnico](#stack-tecnico)
- [Autenticazione](#autenticazione)
- [Database](#database)
- [Design](#design)
- [Stato del progetto](#stato-del-progetto)
- [Nota sullo sviluppo](#nota-sullo-sviluppo)

---

## App disponibili

| App | File | Scopo | Link |
|-----|------|-------|------|
| <img src="https://github.com/Gallo-Giovanni/CasaSmart/raw/main/Screenshot/spesasmart-cover.png" width="80"> SpesaSmart | `SpesaSmart.html` | Digitalizza scontrini, confronta prezzi, gestisce la lista della spesa | [spesasmartt.netlify.app](https://spesasmartt.netlify.app/) |
| <img src="https://github.com/Gallo-Giovanni/CasaSmart/raw/main/Screenshot/salvaspreco-cover.png" width="80"> SalvaSpreco | `SalvaSpreco.html` | Traccia le scadenze dei prodotti alimentari | [salvaspreco.netlify.app](https://salvaspreco.netlify.app/) |

---

## SpesaSmart

SpesaSmart permette a ogni utente di digitalizzare gli scontrini della spesa, salvare i prodotti acquistati, confrontare i prezzi nel tempo e gestire una lista della spesa personale.

**App live:** [spesasmartt.netlify.app](https://spesasmartt.netlify.app/)

### Descrizione

L'app permette a ogni utente di accedere al proprio spazio personale tramite autenticazione e di salvare in modo ordinato i dati dei propri scontrini.

L'obiettivo del progetto è trasformare le informazioni presenti su uno scontrino in dati strutturati, consultabili e modificabili, così da poter monitorare gli acquisti, confrontare i prezzi dei prodotti e semplificare la gestione della spesa.

L'app include anche una schermata introduttiva visibile prima del login, pensata per spiegare in modo immediato le principali funzionalità disponibili e il valore del progetto per l'utente.

### Funzionalità principali

#### Accesso e presentazione iniziale

- Prima dell'autenticazione, l'app mostra una schermata introduttiva con una breve spiegazione delle principali funzionalità disponibili.
- La schermata iniziale presenta in modo sintetico le aree chiave del progetto: inserimento scontrini, confronto prezzi, prodotti preferiti e lista della spesa.
- Al primo accesso dopo un aggiornamento, viene mostrato un **modal changelog** con le novità della versione corrente. La versione vista viene salvata per utente tramite la tabella `user_settings`, così il modal non riappare alle sessioni successive.
- Accesso personale per ogni utente, con gestione separata dei dati tramite account individuale.

#### Inserimento scontrini

- Aggiunta di un nuovo scontrino tramite JSON o inserimento manuale.
- Presenza di un prompt predefinito da copiare e usare su qualsiasi AI esterna.
- L'utente non carica la foto dello scontrino direttamente nell'app: la foto viene scattata o caricata su un servizio AI esterno, che restituisce un JSON strutturato da copiare e incollare successivamente in SpesaSmart.
- Possibilità di inserimento e revisione manuale dei dati.
- Possibilità di assegnare la **categoria** al prodotto direttamente durante l'inserimento o la modifica dello scontrino.

#### Gestione prodotti

- Visualizzazione dei prodotti estratti dagli scontrini, con filtro per categoria e filtro preferiti.
- Salvataggio di informazioni come nome prodotto, prezzo, prezzo scontato, quantità e prezzo per unità o per kg quando disponibile.
- Assegnazione e modifica della categoria in qualsiasi momento (17 categorie con emoji).
- Modifica del nome del prodotto aggiornando tutti gli scontrini in cui appare, mantenendo la categoria.
- **Unione prodotti multipli:** modalità di selezione che permette di scegliere 2 o più prodotti e unirli sotto un unico nome, con salvataggio della cronologia unioni nella tabella `merge_history`.
- **Storico unioni:** bottone dedicato che mostra la lista delle unioni effettuate e permette di ripristinare i nomi originali.

#### Tab Community

- Visualizzazione dei prezzi aggregati registrati dagli altri utenti, esclusi i propri.
- I prodotti mostrano la categoria assegnata dalla community.

#### Modifica scontrino

- Apertura dello scontrino originale associato a un prodotto.
- Modifica di supermercato, data, totale e prodotti contenuti nello scontrino.
- Modifica dei prezzi, delle quantità e di altri dettagli dei prodotti.
- Aggiunta o rimozione manuale di prodotti all'interno dello scontrino.

#### Consultazione scontrini

- Visualizzazione dello storico degli scontrini salvati, raggruppati per anno e mese con totali per periodo espandibili.
- Filtro per supermercato tramite menu a tendina.

#### Analisi prezzi

- Storico degli acquisti per prodotto.
- Confronto dei prezzi registrati nei diversi supermercati.
- Evidenza del prezzo minimo e massimo rilevato.
- Visualizzazione del miglior prezzo disponibile per prodotto.

#### Preferiti

- Sezione dedicata ai prodotti preferiti.
- Possibilità di salvare rapidamente i prodotti usati più spesso.

#### Lista della spesa

- Creazione di una lista della spesa personalizzata.
- Aggiunta manuale dei prodotti da acquistare.
- Collegamento dei prodotti salvati alla lista della spesa.
- Gestione della quantità e dello stato di completamento degli elementi.
- Suggerimento del supermercato più conveniente per alcuni prodotti presenti nella lista.

#### Dashboard iniziale

- Panoramica generale con: numero di scontrini salvati, spesa totale registrata, numero di prodotti unici, numero di supermercati presenti nello storico ed elenco degli ultimi scontrini inseriti.

#### Impostazioni

- Toggle per passare tra **tema scuro e tema chiaro** (salvato in `localStorage`).
- Sezione account con email dell'utente e bottone logout.

### Flusso di utilizzo

1. L'utente apre l'app e visualizza una breve panoramica delle funzionalità principali.
2. Effettua il login.
3. Apre la sezione per aggiungere un nuovo scontrino.
4. Copia il prompt fornito dall'app.
5. Usa il prompt su un servizio AI esterno di sua scelta.
6. Scatta o carica lì la foto dello scontrino.
7. Ottiene un JSON strutturato.
8. Incolla il JSON nell'app.
9. Controlla, modifica e salva i dati estratti.
10. Consulta i prodotti, confronta i prezzi e aggiorna preferiti o lista della spesa.

### Uso dell'AI

L'app **non integra direttamente un modello AI** tramite API o servizi interni.

L'AI viene utilizzata solo in modo esterno al sistema: l'utente copia un prompt fornito dall'app, lo usa su un servizio AI di sua scelta, carica o fotografa lì lo scontrino e ottiene un JSON da incollare successivamente nell'app.

Questo approccio permette di sfruttare strumenti AI per l'estrazione e la prima strutturazione dei dati, mentre la gestione, il salvataggio e la modifica finale delle informazioni avvengono interamente all'interno dell'app.

---

## SalvaSpreco

SalvaSpreco aiuta gli utenti a tenere traccia delle scadenze dei prodotti alimentari e non, con avvisi visivi e notifiche per prodotti in scadenza o già scaduti.

Condivide lo stesso progetto Supabase di SpesaSmart: chi è già registrato su una delle due app può accedere direttamente anche all'altra senza una nuova registrazione.

**App live:** [salvaspreco.netlify.app](https://salvaspreco.netlify.app/)

### Funzionalità principali

#### Tab Prodotti — schermata principale

- Form di inserimento con: nome, data scadenza, tipo scadenza, categoria, numero di pezzi, quantità prodotto e unità di misura (gr / kg / lt / ml / pz).
- **Tipo scadenza:** ogni prodotto può essere marcato come `⛔ Entro il` (scadenza tassativa) oppure `💛 Preferibilmente entro` (TMC — termine minimo di conservazione). Questa distinzione viene mostrata visivamente nelle card.
- **Classificazione automatica della categoria** — al blur del campo nome, una funzione locale basata su centinaia di parole chiave seleziona automaticamente la categoria più adatta, senza chiamate API esterne. Accanto alla label appare un badge `✦ auto`. La categoria può essere modificata manualmente in qualsiasi momento.
- **4 contatori colorati** aggiornati in tempo reale: Scaduti / Scadenza entro 3 giorni / Scadenza entro 7 giorni / Oltre 7 giorni.
- Lista prodotti ordinata per data di scadenza, con card colorate in base allo stato.
- **Doppio filtro combinato:** una prima riga di chip filtra per stato di scadenza (tutti / scaduti / 3gg / 7gg / ok), una seconda filtra per categoria (15 categorie con emoji). I due filtri si combinano — è possibile vedere, ad esempio, solo i Latticini in scadenza entro 3 giorni.
- Sui prodotti già scaduti: bottone archivio (sposta in storico) e bottone elimina definitivo.
- **Archiviazione automatica** dopo 14 giorni dalla scadenza, all'apertura dell'app.

#### Tab Storico

Due sotto-tab:

- **Scaduti** — Top 5 prodotti più scaduti con barra proporzionale; lista completa dei prodotti archiviati con data rimozione, giorni di ritardo alla rimozione (verde / arancione / rosso), categoria e quantità.
- **Eliminati** — lista dei prodotti rimossi manualmente prima della scadenza, con possibilità di eliminazione definitiva o ripristino.

#### Notifiche in-app

- **Banner al login** — ogni volta che si apre l'app, un banner colorato appare in cima alla lista prodotti. Mostra una riga rossa per i prodotti già scaduti (con i nomi) e una arancione per quelli in scadenza entro 3 giorni. Ogni riga ha una X per chiuderla; la chiusura viene salvata per evitare che il banner riappaia nella stessa giornata.

#### Web Push

- Al primo accesso, un modal overlay chiede il permesso per le notifiche di sistema.
- Funziona su desktop; su mobile richiede che l'app sia servita via HTTPS (es. Netlify).

#### Tab Impostazioni

- Toggle on/off per il **tema scuro / chiaro** — preferenza salvata su `user_settings` per persistenza cross-device.
- Toggle on/off per le notifiche push.
- **Orario notifica giornaliera** — selezione dell'ora del giorno in cui ricevere il promemoria (dalle 06:00 alle 22:00).
- **Stile notifica** — scelta tra `Breve` (mostra solo il numero di prodotti) e `Dettagliata` (elenca anche i nomi).
- Riga di stato che mostra se le notifiche sono Attive / Disattive / Bloccate dal browser.
- Sezione account con email dell'utente e bottone logout.

---

## Stack tecnico

- HTML / CSS / JavaScript puro, nessun framework
- **Supabase** per autenticazione e database (progetto condiviso tra le due app)
- **Netlify** per il deploy del frontend
- Tabler Icons + DM Sans / DM Mono via CDN

---

## Autenticazione

- Login e registrazione con email/password tramite Supabase Auth.
- **Account condiviso** tra SpesaSmart e SalvaSpreco: stesso progetto Supabase, sessione persistente.
- Row Level Security attiva su tutte le tabelle: ogni utente vede esclusivamente i propri dati.

---

## Database

L'app utilizza Supabase come backend per la gestione dei dati e dell'autenticazione.

### Tabelle di SpesaSmart

- `scontrini` → contiene `supermercato`, `data`, `prodotti` (jsonb), `totale_scontrino`, `created_at`, `user_id`.
- `preferiti` → memorizza i prodotti preferiti dell'utente (`id`, `user_id`, `nome_prodotto`, `created_at`).
- `lista_spesa` → salva i prodotti da acquistare (`id`, `user_id`, `nome_prodotto`, `quantita`, `completato`, `created_at`).
- `prodotti_categorie` → associa i prodotti alle categorie personalizzate (`id`, `user_id`, `nome_prodotto`, `categoria`, `created_at`).
- `prodotti_community` → raccoglie prezzi aggregati da tutti gli utenti (`id`, `nome_prodotto`, `supermercato`, `prezzo`, `conteggio`, `utenti_unici`, `ultima_data`, `created_at`, `user_id`, `categoria`).
- `merge_history` → cronologia delle unioni di prodotti (`id`, `user_id`, `nome_finale`, `nomi_originali`, `created_at`).
- `user_settings` → preferenze utente cross-device (`id`, `user_id`, `versione_vista`, `versione_vista_scontrini`, `orario_notifica`, `notifica_stile`, `notifica_soglia`, `created_at`, `updated_at`).
- `push_subscriptions` → sottoscrizioni Web Push (`id`, `user_id`, `subscription` jsonb, `created_at`).

### Tabelle di SalvaSpreco

- `prodotti_scadenze` → `id`, `user_id`, `nome`, `data_scadenza`, `categoria`, `stato` (`attivo` / `scaduto_eliminato` / `eliminato`), `rimosso_il`, `giorni_scaduto_al_momento`, `quantita`, `unita_misura`, `quantita_prodotto`, `banner_chiuso_il`, `tipo_scadenza` (`entro` / `preferibilmente`), `created_at`.

Tutte le tabelle sono collegate all'utente tramite `user_id`, con riferimento al sistema di autenticazione di Supabase (`auth.users.id`).

[![Schema database](https://github.com/Gallo-Giovanni/CasaSmart/raw/main/Screenshot/Database.png)](https://github.com/Gallo-Giovanni/CasaSmart/blob/main/Screenshot/Database.png)

---

## Design

Entrambe le app condividono lo stesso sistema visivo:

- Tema scuro (default `#0f0f0f`) e tema chiaro, selezionabili dall'utente nelle impostazioni.
- Accent viola `#6C63FF`.
- Schermata di login con feature card introduttive e card "Account condiviso" che menziona entrambe le app.

---

## Stato del progetto

Il progetto è in continuo sviluppo. Nuove funzionalità, miglioramenti all'interfaccia e ottimizzazioni al database vengono rilasciati progressivamente.

---

## Nota sullo sviluppo

Questo progetto è stato ideato, costruito e migliorato con il supporto dell'AI.

L'AI è stata utilizzata come strumento di supporto durante lo sviluppo: nella generazione di idee, nel debugging, nella scrittura e revisione di parti del codice e nella definizione di alcune soluzioni implementative.

Le scelte progettuali, l'adattamento delle funzionalità, le modifiche finali e l'organizzazione complessiva del progetto sono state gestite direttamente dall'autore.
