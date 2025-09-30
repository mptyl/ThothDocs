# Colonne SQL

Questo documento descrive i metadati relativi alle colonne SQL gestite all'interno del sistema Thoth. I metadati sono informazioni strutturate che descrivono le colonne e facilitano la gestione e l'interrogazione dei dati.

## 1 - Modello SqlColumn

### 1.1 - Descrizione
Il modello `SqlColumn` descrive una colonna specifica all'interno di una tabella di un database SQL. Questo livello di dettaglio è essenziale per comprendere la struttura dei dati.

- **Campi Principali**:
  - **Nome Colonna Originale**: Il nome originale della colonna nel database.
  - **Nome Colonna**: Un nome espanso o modificato per chiarezza, se necessario.
  - **Formato Dati**: Il tipo di dato della colonna (ad esempio, INT, VARCHAR, DATE).
  - **Descrizione Colonna**: Una descrizione testuale del contenuto o dello scopo della colonna.
  - **Commento Generato**: Commenti aggiuntivi generati automaticamente, spesso tramite AI.
  - **Descrizione Valori**: Informazioni sui valori tipici o sul significato dei dati nella colonna.
  - **Campo PK**: Indica se la colonna è una chiave primaria.
  - **Campo FK**: Specifica se la colonna è una chiave esterna e a quale tabella/colonna fa riferimento.
  - **Tabella SQL**: Riferimento alla tabella (`SqlTable`) a cui appartiene la colonna.

- **Relazioni**:
  - Una `SqlColumn` appartiene a una sola `SqlTable`.
  - Può essere coinvolta in `Relationship` come colonna di origine o di destinazione per definire relazioni di chiave esterna.

## 2 - Utilizzo del Modello in ThothSL

Nel progetto ThothSL, il modello `SqlColumn` viene utilizzato per fornire informazioni dettagliate sulle colonne durante la generazione di query SQL. Le funzioni principali includono:

- **Recupero dello Schema del Database**: Il sistema estrae la struttura delle colonne per avere una visione d'insieme dei dati disponibili.
- **Generazione di Stringhe di Schema**: Le informazioni sulle colonne vengono convertite in rappresentazioni testuali ottimizzate per l'uso in prompt di modelli linguistici.
- **Filtraggio dello Schema**: Vengono selezionate solo le colonne rilevanti per una specifica query o contesto, riducendo la complessità.
- **Arricchimento con Esempi di Valori**: Quando disponibili, vengono aggiunti esempi di valori per le colonne, fornendo un contesto aggiuntivo per la generazione di query.

## 3 - Form di Amministrazione Associato

### 3.1 - SqlColumnAdmin
Il form di amministrazione per `SqlColumn` gestisce i dettagli delle colonne all'interno delle tabelle.

- **Funzionalità Gestite**:
  - Modifica del nome originale e del nome espanso della colonna.
  - Specifica del tipo di dato.
  - Inserimento di descrizioni e commenti generati.
  - Indicazione di chiavi primarie ed esterne.
  - Generazione di commenti per le colonne selezionate tramite AI.
  - Copia dei nomi e dei commenti tra campi diversi.
