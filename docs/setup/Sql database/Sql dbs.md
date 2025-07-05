# Database SQL

Questo documento descrive i metadati relativi ai database SQL gestiti all'interno del sistema Thoth. I metadati sono informazioni strutturate che descrivono i database e facilitano la gestione e l'interrogazione dei dati.

## 1 - Modello SqlDb

### 1.1 - Descrizione
Il modello `SqlDb` rappresenta un database SQL all'interno del sistema. Contiene le informazioni di base necessarie per connettersi e interagire con un database specifico.

- **Campi Principali**:
  - **Nome**: Un identificativo univoco per il database.
  - **Host del Database**: L'indirizzo del server che ospita il database.
  - **Tipo di Database**: Specifica il tipo di database, come PostgreSQL, MySQL, SQLite, ecc.
  - **Nome del Database**: Il nome effettivo del database sul server.
  - **Porta del Database**: La porta di connessione al database.
  - **Schema**: Lo schema specifico all'interno del database, se applicabile.
  - **Nome Utente e Password**: Credenziali per accedere al database.
  - **Modalità del Database**: Indica l'ambiente del database (sviluppo, test, produzione).
  - **Database Vettoriale**: Un riferimento opzionale a un database vettoriale associato per funzionalità avanzate.
  - **Lingua**: La lingua predefinita per il database.
  - **Ambito**: Una descrizione testuale dell'ambito o del contesto del database.
  - **Ultimo Aggiornamento Colonne**: Data e ora dell'ultimo aggiornamento delle informazioni sulle colonne.

- **Relazioni**:
  - Un `SqlDb` può avere più `SqlTable` associate, rappresentando le tabelle contenute nel database.
  - È collegato opzionalmente a un `VectorDb` per funzionalità di ricerca vettoriale.

## 2 - Utilizzo del Modello in ThothSL

Nel progetto ThothSL, il modello `SqlDb` viene utilizzato per facilitare la connessione ai database e la gestione dei metadati per la generazione di query SQL. Le funzioni principali includono:

- **Recupero dello Schema del Database**: Il sistema estrae la struttura completa del database per avere una visione d'insieme dei dati disponibili.

## 3 - Form di Amministrazione Associato

### 3.1 - SqlDbAdmin
Il form di amministrazione per `SqlDb` consente di gestire le informazioni di connessione e configurazione di un database.

- **Funzionalità Gestite**:
  - Configurazione dei dettagli di connessione come host, porta, nome utente e password.
  - Selezione del tipo di database e dello schema.
  - Associazione con un database vettoriale.
  - Azioni per creare tabelle, relazioni e altri elementi del database.
  - Esportazione della struttura del database in formato CSV.
  - Generazione di un ambito descrittivo per il database.
