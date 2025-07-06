# Database SQL

Questo manuale utente fornisce una panoramica sul sistema di gestione dei database SQL all'interno di Thoth AI Backend. Qui troverai informazioni su come configurare, gestire e utilizzare i database SQL per supportare le funzionalità del sistema.

## 1 - Introduzione ai Database SQL in Thoth

Thoth AI Backend offre un sistema avanzato per la gestione di database SQL, permettendo agli utenti di configurare e amministrare diversi tipi di database relazionali. Questo sistema è progettato per integrarsi con le altre componenti del backend, facilitando l'accesso ai dati e la loro elaborazione.

### 1.1 - Scopo del Sistema di Gestione dei Database

Il sistema di gestione dei database SQL in Thoth ha l'obiettivo di fornire un'interfaccia intuitiva per la configurazione e l'amministrazione dei database. Consente agli utenti di definire le connessioni ai database, gestire le tabelle e le colonne, e stabilire relazioni tra i dati, il tutto attraverso un'interfaccia di amministrazione user-friendly.

### 1.2 - Tipologie di Database Supportate

Thoth supporta una vasta gamma di database SQL, inclusi sistemi popolari come PostgreSQL, MySQL, MariaDB, Oracle, SQL Server e SQLite. Questo permette agli utenti di scegliere il database più adatto alle loro esigenze specifiche, garantendo flessibilità e compatibilità.

## 2 - Configurazione di un Database SQL

La configurazione di un database SQL in Thoth avviene attraverso un form di amministrazione che raccoglie tutte le informazioni necessarie per stabilire una connessione e gestire il database.

### 2.1 - Informazioni di Base

- **Nome del Database**: Un identificativo univoco per il database all'interno del sistema Thoth.
- **Tipo di Database**: La selezione del tipo di database (ad esempio, PostgreSQL, MySQL, ecc.) che determina le opzioni di configurazione specifiche.
- **Host del Database**: L'indirizzo del server su cui risiede il database.
- **Nome della Base di Dati**: Il nome specifico del database sul server.
- **Schema**: Lo schema utilizzato all'interno del database, se applicabile.
- **Porta del Database**: La porta di connessione al server del database.
- **Credenziali di Accesso**: Nome utente e password per accedere al database.
- **Modalità del Database**: Indica se il database è utilizzato in ambiente di sviluppo, test o produzione.

### 2.2 - Integrazione con Database Vettoriali

Ogni database SQL può essere associato a un database vettoriale per funzionalità avanzate di ricerca e analisi dei dati. Questa integrazione permette di combinare i dati strutturati del database SQL con le capacità di ricerca vettoriale.

### 2.3 - Impostazioni Avanzate

- **Lingua**: La lingua utilizzata per i dati e i metadati del database, utile per la generazione di contenuti localizzati.
- **Ambito**: Una descrizione testuale dell'ambito o del contesto del database, che può essere generata automaticamente o inserita manualmente.

## 3 - Gestione delle Tabelle e delle Colonne

Una volta configurato il database, è possibile gestire le tabelle e le colonne che contengono i dati.

### 3.1 - Creazione e Gestione delle Tabelle

Le tabelle rappresentano le entità principali all'interno di un database SQL. Thoth permette di creare tabelle automaticamente o manualmente, con le seguenti funzionalità:

- **Nome della Tabella**: Un identificativo per la tabella.
- **Descrizione**: Una spiegazione del contenuto o dello scopo della tabella.
- **Commento Generato**: Un commento descrittivo generato automaticamente, spesso con l'assistenza dell'AI, che può essere copiato nella descrizione.

### 3.2 - Creazione e Gestione delle Colonne

Le colonne definiscono i campi di dati all'interno di ogni tabella. Per ogni colonna, è possibile specificare:

- **Nome Originale della Colonna**: Il nome della colonna così come appare nel database.
- **Nome della Colonna**: Un nome personalizzato per la colonna, se necessario.
- **Formato dei Dati**: Il tipo di dato della colonna (ad esempio, intero, stringa, data, ecc.).
- **Descrizione della Colonna**: Una spiegazione del contenuto della colonna.
- **Commento Generato**: Un commento descrittivo generato automaticamente, spesso con l'assistenza dell'AI.
- **Descrizione del Valore**: Informazioni aggiuntive sul significato dei valori nella colonna.
- **Campo Chiave Primaria**: Indica se la colonna è una chiave primaria.
- **Campo Chiave Esterna**: Specifica eventuali riferimenti a colonne di altre tabelle come chiavi esterne.

## 4 - Relazioni tra Tabelle

Thoth consente di definire relazioni tra tabelle per rappresentare i collegamenti tra i dati.

### 4.1 - Configurazione delle Relazioni

Le relazioni sono definite specificando una tabella e una colonna di origine, e una tabella e una colonna di destinazione. Questo permette di stabilire collegamenti logici tra i dati, come chiavi esterne che puntano a chiavi primarie in altre tabelle.

### 4.2 - Aggiornamento dei Campi Chiave

Dopo aver definito una relazione, è possibile aggiornare automaticamente i campi di chiave primaria e esterna nelle colonne coinvolte, semplificando la gestione delle dipendenze tra tabelle.

## 5 - Azioni Amministrative Disponibili

L'interfaccia di amministrazione di Thoth offre una serie di azioni per gestire i database SQL in modo efficiente.

### 5.1 - Esportazione e Importazione dei Dati

- **Esportazione in CSV**: Permette di esportare i dati di database, tabelle, colonne o relazioni in file CSV per backup o analisi esterne.
- **Importazione da CSV**: Consente di importare dati da file CSV per popolare o aggiornare il database.
- **Esportazione della Struttura del Database**: Esporta la struttura completa del database in file CSV per documentazione o migrazione.

### 5.2 - Generazione di Commenti Assistita da AI

- **Generazione di Commenti per Tabelle**: Crea descrizioni automatiche per le tabelle basate sul loro contenuto e scopo, con l'aiuto dell'intelligenza artificiale.
- **Generazione di Commenti per Colonne**: Genera descrizioni dettagliate per le colonne, anch'esse assistite da AI, che possono essere copiate nelle descrizioni ufficiali.

### 5.3 - Validazione dei Campi di Chiave Esterna

- **Validazione a Livello di Database**: Controlla la correttezza dei riferimenti di chiave esterna in tutte le tabelle di un database selezionato, segnalando eventuali errori di formato o riferimenti inesistenti.
- **Validazione a Livello di Tabella**: Esegue la validazione dei campi di chiave esterna solo per le tabelle selezionate.
- **Validazione a Livello di Colonna**: Verifica i riferimenti di chiave esterna per le colonne selezionate, garantendo che siano correttamente formattati e validi.

### 5.4 - Pulizia dei Campi Chiave Non Validi

- **Pulizia dei Campi Chiave Primaria/Esterna Non Validi**: Rimuove o corregge i valori non validi nei campi di chiave primaria ed esterna delle tabelle selezionate, mantenendo solo i riferimenti corretti.

### 5.5 - Copia dei Commenti Generati

- **Copia dei Commenti Generati nelle Descrizioni**: Trasferisce i commenti generati automaticamente nelle descrizioni ufficiali di tabelle e colonne, solo se contengono testo significativo.

### 5.6 - Gestione dei Nomi delle Colonne

- **Copia del Nome Originale nel Nome della Colonna**: Sostituisce il nome personalizzato della colonna con il nome originale dal database.
- **Copia del Nome della Colonna nel Nome Originale**: Sostituisce il nome originale con il nome personalizzato della colonna.

### 5.7 - Creazione di Elementi del Database

- **Creazione delle Tabelle**: Genera automaticamente le tabelle per un database selezionato, basandosi sulle informazioni disponibili.
- **Creazione delle Colonne**: Popola le tabelle con le colonne corrispondenti, identificate dal sistema.
- **Creazione delle Relazioni**: Identifica e crea relazioni tra tabelle basate sui dati e le chiavi esterne.
- **Creazione Completa degli Elementi del Database**: Esegue un processo completo di creazione di tabelle, colonne e relazioni per un database selezionato.

### 5.8 - Generazione dell'Ambito del Database

- **Generazione dell'Ambito**: Crea una descrizione testuale dell'ambito o del contesto del database, utile per comprendere il suo utilizzo e contenuto.

## 6 - Conclusione

Il sistema di gestione dei database SQL in Thoth AI Backend è progettato per semplificare la configurazione, l'amministrazione e l'utilizzo dei database relazionali. Con un'interfaccia intuitiva e una serie di azioni avanzate, gli utenti possono gestire efficacemente i loro dati, garantendo che siano ben strutturati e accessibili per le esigenze del sistema.
