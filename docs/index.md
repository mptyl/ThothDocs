# Benvenuti in ThothAI
**ThothAI** e un'applicazione che permette, utilizzando l'AI, di produrre le adeguate istruzioni SQL in grado estrarre le informazioni richieste da un database relazionale.
Le applicazioni di questo tipo sono denominate `Text-to-SQL`

![Lista containers](assets/index_pngs/FrontendWindow.png)

Ciò che caratterizza **ThothAI** è:

- la disponibilità di una **interfaccia utente** (`ThothSL`) molto semplice da usare e allo stesso tempo molto adattabile;
- la possibilità di indicare, tra i parametri di configurazione:

    1. gli **utenti** abilitati all'uso dell'applicazione e le loro autorizzazioni;
    2. i **database da interrogare** e i **database vettoriali** da associare per la conservazione dei matadati necessari per la trasformazione della richiesta in SQL;
    3. i **modelli LLM** da utilizzare;

- l'utilizzo di un'applicazione specifica di backend per la gestione:

    1. dei **parametri di configurazione**;
    2. dei **metadati** del database da interrogare (tables, columns, relationships);
    3. delle **descrizioni di tabelle e colonne**, che, se non disponibili, possono essere generate tramite AI;
    4. del **preprocessing** del database da esaminare per creare hash e vettori che facilitino il processo di generazione del SQL;
    5. di **hints** che possano chiarire termini complessi o suggerire interpretazioni non intuitive e, soprattutto, non facilmente ricavabili dai nomi dei campi e dalle descrizioni di campi e tabelle;
    6. di coppie **question/SQL** sicuramente corrette che possano costituire da guida ed esempio per le richieste che verranno effettuate in futuro;

- l'utilizzo di un **database vettoriale** per la conservazione degli **hints** e delle coppie **question/SQL**, che può essere alimentato dagli utenti autorizzati;
- l'ampio uso di **LLM**, anche di piccola o media dimensione, per effettuare le varie fasi del workflow di generazione del SQL da eseguire. 
ThothAI permette di definire su database gli attributi degli **LLM** da utilizzare, rendendo semplice l'adeguamento dell'applicazione ai futuri sviluppi;
- la possibilità di adattare il processo di generazione alla complessità e alla dimensione del proprio schema di database.
Con **ThothAI**, infatti, si può scegliere tra due approcci:
    1. un `workflow incrementale`, che prova prima con modelli piccoli ed economici, facendo poi escalation su modelli più grandi in caso di insuccesso. 
Per poter utilizzare modelli piccoli, con una tecnica RAG, dallo schema totale vengono prima estratte solo le tabelle e le colonne che hanno elevata probabilità di essere oggetto del SQL;
    2. un `workflow basato sulla forza bruta`che utilizza immediatamente un modello potente, lasciando che sia lui a fare quasi tutto il lavoro a partire dallo schema con tutte le tabelle e le colonne disponibili.

## 1 - Come utilizzare ThothAI
1. Seguire le [istruzioni di installazione](1-install/1.1-sources_cloning.md).
2. Rendere confidenza con l'applicazione utilizzando il [Quick Start](2-quickstart/2.1-quickstart_frontend.md)
3. Leggere la pagina dello **User Manual** dedicata a una [panoramica sul processo di setup](3-user_manual/3.1-setup/3.1.0-setup_process.md)
4. Configurare l'applicazione configurando prima di tutto i propri [gruppi](3-user_manual/3.1-setup/3.1.1-authentication/3.1.1.1-groups.md) e i propri [utenti](3-user_manual/3.1-setup/3.1.1-authentication/3.1.1.2-users.md/)
5. Modificare, se necessario, la lista dei [modelli di AI](3-user_manual/3.1-setup/3.1.2-AI_models_and_agents/3.1.2.2-ai_models.md) (LLM) da utilizzare nell'esecuzione del processo
6. Adeguare, se necessario, gli Agent configurandoli come descritto in [questa pagina](3-user_manual/3.1-setup/3.1.2-AI_models_and_agents/3.1.2.3-agents.md)
7. Impostare il [database vettoriale](3-user_manual/3.1-setup/3.1.3-vector_database/3.1.3.1-vector_db.md) destinato a contenere i metadati del database relazionale da interrogare
8. Impostare i parametri per la [configurazione del database](3-user_manual/3.1-setup/3.1.4-SQL_database/3.1.4.1-sql_dbs.md) da interrogare e completarne la descrizione di dettaglio con tables, columns, relationnships, commenti e scope
9. Impostare un [Setting](3-user_manual/3.1-setup/3.1.0-setup_process.md) specifico per l'attività che si vuole condurre nel caso quello di Default non dovesse essere adeguato
10. Impostare un il [Workflow](3-user_manual/3.1-setup/3.1.6.1-workspaces.md) per connettere un insieme di utenti, un database da interrogare, un insieme di Agent da utilizzare e un Setting 
11. Eseguire le attività di [Preprocessing](3-user_manual/3.2-preprocessing/3.2.1-why_the_preprocessing.md) del database 
12. Andare sul frontend all'indirizzo [http://localhost:8501](http://localhost:8501) (porta 8503 se si sta lavorando in locale) e operare come indicato nelle seguenti brevi [istruzioni](3-user_manual/3.9-frontend.md)

## 2 - I log delle attività svolte
Esaminare quanto indicato nella [pagina dedicata al Log Management](3-user_manual/3.4-logging/3.4.2-log_management.md)

## 3 - La Roadmap
La Roadmap di sviluppo di **ThothAI** è [qui descritta](3-user_manual/3.8-roadmap.md)

## 4 - Riferimenti a prodotti e papers
La pagina sui [Riferimenti](references.md) raccoglie  i prodotti, gli studi e i papers che hanno ispirato **ThothAI**

## 5 - Il Reference Manual
Approfondimenti tecnici sono disponibili nel [Reference Manual](4-reference_manual/4.1-reference_manual_map.md)

## 6 - Cos'e il Text-to-SQL
Per approfondimento sulle tecniche raccolte sotto il nome `Text-to-SQL` leggere [questa pagina](text-to-SQL.md)
  


