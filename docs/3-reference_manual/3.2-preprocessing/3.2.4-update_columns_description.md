# Aggiornamento delle Descrizioni delle Colonne

La funzionalità **Aggiornamento delle Descrizioni delle Colonne** nel backend di Thoth consente di aggiornare le informazioni relative alle colonne di un database associato a un workspace. Questa operazione è utile per mantenere aggiornate le descrizioni dei campi, specialmente quando si lavora con database che richiedono metadati dettagliati, come quelli provenienti da BIRD.

## 1 - Cosa Succede Quando si Preme "Aggiornamento delle Descrizioni delle Colonne"

Quando si preme il pulsante "Aggiornamento delle Descrizioni delle Colonne" nell'area di preprocessing del backend di Thoth, viene avviato un processo che aggiorna i metadati delle colonne del database configurato per il workspace corrente. Il sistema legge informazioni da file CSV specifici, organizzati in base al nome e alla modalità del database (ad esempio, sviluppo, test o produzione), e utilizza questi dati per aggiornare le descrizioni delle colonne direttamente nel database.

Durante l'operazione, un indicatore di caricamento (spinner) fornisce un feedback visivo sullo stato del processo. Una volta completato, viene visualizzato un messaggio di conferma che indica il successo dell'aggiornamento, insieme alla data e all'ora dell'ultimo aggiornamento.

## 2 - Informazioni Caricate

Le informazioni caricate durante questo processo includono:
- Nomi delle colonne, che possono essere aggiornati o mantenuti come esistenti.
- Descrizioni delle colonne, che forniscono dettagli sul significato o sull'uso di ciascun campo.
- Descrizioni dei valori, che offrono ulteriori chiarimenti sui dati contenuti nelle colonne.

Questi dati provengono da file CSV predefiniti, strutturati per corrispondere alle tabelle del database, garantendo che le informazioni siano correttamente associate alle colonne esistenti.

## 3 - Come Vengono Caricate le Colonne

Nel backend di Thoth, che condivide il database con ThothSL, le colonne vengono aggiornate confrontando i dati dei file CSV con le colonne già presenti nel database. Se una corrispondenza viene trovata, i metadati della colonna vengono aggiornati con le nuove informazioni. Questo processo assicura che le descrizioni siano sempre accurate e riflettano le specifiche più recenti del database, migliorando la comprensione dei dati sia per gli utenti del backend di Thoth che per quelli di ThothSL.

## 4 - Parte della UI Responsabile del Caricamento delle Colonne

La sezione di **Preprocessing** dell'interfaccia utente del backend di Thoth è responsabile della gestione di questa funzionalità. All'interno di questa area, un pulsante dedicato denominato "Aggiornamento delle Descrizioni delle Colonne" permette di avviare il processo di aggiornamento. L'interfaccia mostra anche la data dell'ultimo aggiornamento effettuato, o un messaggio che indica se l'operazione non è mai stata eseguita. Durante l'elaborazione, un indicatore di caricamento appare per segnalare che l'operazione è in corso, e al termine vengono mostrati messaggi di successo o eventuali errori, garantendo un'esperienza utente chiara e informativa.
