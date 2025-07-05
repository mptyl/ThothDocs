# Relazioni SQL

Questo documento descrive i metadati relativi alle relazioni SQL gestite all'interno del sistema Thoth. I metadati sono informazioni strutturate che descrivono le relazioni tra tabelle e facilitano la gestione e l'interrogazione dei dati.

## 1 - Modello Relationship

### 1.1 - Descrizione
Il modello `Relationship` definisce le relazioni tra tabelle attraverso le chiavi esterne, evidenziando i collegamenti logici tra i dati.

- **Campi Principali**:
  - **Tabella di Origine**: La tabella che contiene la chiave esterna.
  - **Tabella di Destinazione**: La tabella a cui la chiave esterna fa riferimento.
  - **Colonna di Origine**: La colonna nella tabella di origine che agisce come chiave esterna.
  - **Colonna di Destinazione**: La colonna nella tabella di destinazione referenziata dalla chiave esterna.

- **Relazioni**:
  - Collega due `SqlTable` (origine e destinazione) e due `SqlColumn` (origine e destinazione) per rappresentare una relazione di chiave esterna.

## 2 - Utilizzo del Modello in ThothSL

Nel progetto ThothSL, il modello `Relationship` viene utilizzato per mappare le connessioni tra tabelle durante la generazione di query SQL. Le funzioni principali includono:

- **Recupero dello Schema del Database**: Il sistema estrae le relazioni tra tabelle per avere una visione d'insieme dei dati disponibili.
- **Generazione di Stringhe di Schema**: Le informazioni sulle relazioni vengono convertite in rappresentazioni testuali ottimizzate per l'uso in prompt di modelli linguistici.

## 3 - Form di Amministrazione Associato

### 3.1 - RelationshipAdmin
Il form di amministrazione per `Relationship` permette di definire e gestire le relazioni tra tabelle.

- **Funzionalità Gestite**:
  - Selezione delle tabelle e delle colonne di origine e destinazione per definire una relazione.
  - Aggiornamento dei campi di chiave primaria ed esterna nelle colonne coinvolte.
  - Filtri per visualizzare relazioni specifiche in base al database o alla tabella.
