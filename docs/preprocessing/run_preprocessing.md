# Esecuzione del Preprocessing

## 1 - Introduzione
Il preprocessing è un passaggio fondamentale nell'applicazione Thoth per preparare i dati del database per analisi e operazioni avanzate. Questa sezione del manuale utente descrive cosa succede quando si esegue il preprocessing, quali informazioni vengono create e salvate, come vengono gestite nel backend e come l'interfaccia utente (UI) di Thoth facilita questo processo.

## 2 - Cosa Succede Durante il Preprocessing
Quando si avvia il processo di "Run Preprocessing" nell'applicazione Thoth, viene avviata una sequenza di operazioni che preparano il database per un'elaborazione efficiente. Il processo include:

- **Creazione di firme LSH (Locality-Sensitive Hashing)**: Queste firme consentono ricerche di similarità rapide ed efficienti tra gli elementi del database.
- **Generazione di vettori di contesto**: I vettori di contesto rappresentano informazioni semantiche relative alle colonne del database, che vengono poi utilizzati per migliorare la comprensione e l'interrogazione dei dati.

Questo processo viene eseguito in background per non bloccare l'interfaccia utente, permettendo di continuare a lavorare mentre il preprocessing è in corso.

## 3 - Informazioni Create e Salvate
Durante l'esecuzione del preprocessing, vengono generate due tipologie principali di dati:

- **Firme LSH**: Queste sono rappresentazioni compatte degli elementi del database che facilitano il confronto rapido per trovare dati simili. Sono salvate in una struttura di directory sul file system.
- **Vettori di Contesto**: Questi vettori catturano il significato e il contesto delle colonne del database, utili per interrogazioni avanzate e analisi. Sono memorizzati in un database vettoriale associato.

## 4 - Come Vengono Salvate le Informazioni nel Backend
Le informazioni generate durante il preprocessing vengono salvate in modo strutturato nel backend di Thoth, che funge da supporto per ThothSL, condividendo i risultati del preprocessing:

- **Firme LSH**: Sono archiviate in una directory specifica sul file system, organizzata in base alla modalità del database (ad esempio, "prod" o "dev") e al nome del database. Il percorso tipico è del tipo `<radice_lsh>/<modalità_database>_databases/<nome_database>`.
- **Vettori di Contesto**: Sono salvati in un database vettoriale (come Qdrant), configurato in base alle impostazioni del database relazionale associato nel sistema Thoth. Questo permette un accesso rapido e scalabile ai dati di contesto.

## 5 - Interfaccia Utente per il Preprocessing
L'interfaccia utente di Thoth offre un modo semplice e intuitivo per avviare e monitorare il processo di preprocessing:

- **Pagina di Preprocessing**: Una sezione dedicata dell'applicazione, accessibile tramite la vista "Preprocess", mostra informazioni sul workspace corrente, il database associato e lo stato del preprocessing. Qui è presente un pulsante "Run Preprocessing" per avviare il processo.
- **Esecuzione in Background**: Una volta avviato, il preprocessing viene eseguito in un thread separato, evitando di bloccare l'interfaccia. Durante l'elaborazione, l'utente riceve aggiornamenti sullo stato tramite un meccanismo di polling che mostra se il processo è ancora in corso o completato.
- **Feedback sullo Stato**: Al termine del preprocessing, l'interfaccia fornisce un feedback sul risultato dell'operazione, indicando se è stata completata con successo o se si sono verificati errori, insieme a eventuali messaggi di log per ulteriori dettagli.

Questa integrazione tra backend e interfaccia utente assicura che gli utenti possano gestire facilmente il preprocessing senza dover interagire direttamente con i dettagli tecnici del processo.
