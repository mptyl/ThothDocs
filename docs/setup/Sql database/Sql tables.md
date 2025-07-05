# Tabelle SQL

Questo documento descrive i metadati relativi alle tabelle SQL gestite all'interno del sistema Thoth. I metadati sono informazioni strutturate che descrivono le tabelle e facilitano la gestione e l'interrogazione dei dati.

## 1 - Modello SqlTable

### 1.1 - Descrizione
Il modello `SqlTable` rappresenta una tabella all'interno di un database SQL. Serve a catalogare le tabelle disponibili per l'interrogazione.

- **Campi Principali**:
  - **Nome**: Il nome della tabella.
  - **Descrizione**: Una descrizione testuale della tabella e del suo scopo.
  - **Commento Generato**: Commenti generati automaticamente, spesso tramite AI, per descrivere la tabella.
  - **Database SQL**: Riferimento al database (`SqlDb`) a cui appartiene la tabella.

- **Relazioni**:
  - Una `SqlTable` appartiene a un solo `SqlDb`.
  - Può contenere più `SqlColumn`, che rappresentano le colonne della tabella.
  - È coinvolta in `Relationship` come tabella di origine o di destinazione per definire relazioni tra tabelle.

## 2 - Utilizzo del Modello in ThothSL

Nel progetto ThothSL, il modello `SqlTable` viene utilizzato per organizzare e accedere alle informazioni sulle tabelle durante la generazione di query SQL. Le funzioni principali includono:

- **Recupero dello Schema del Database**: Il sistema estrae la struttura delle tabelle per avere una visione d'insieme dei dati disponibili.
- **Generazione di Stringhe di Schema**: Le informazioni sulle tabelle vengono convertite in rappresentazioni testuali ottimizzate per l'uso in prompt di modelli linguistici.
- **Filtraggio dello Schema**: Vengono selezionate solo le tabelle rilevanti per una specifica query o contesto, riducendo la complessità.

## 3 - Form di Amministrazione Associato

### 3.1 - SqlTableAdmin
Il form di amministrazione per `SqlTable` permette di visualizzare e modificare le informazioni sulle tabelle di un database.

- **Funzionalità Gestite**:
  - Visualizzazione e modifica del nome e della descrizione della tabella.
  - Generazione di commenti per la tabella tramite AI.
  - Creazione di colonne per la tabella.
  - Validazione dei campi di chiave primaria ed esterna.
  - Copia dei commenti generati nella descrizione.
