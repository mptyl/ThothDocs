# Domande (Questions)

## 1 - Introduzione

Le **Domande** (Questions) rappresentano un elemento centrale nell'applicazione ThothSL, che utilizza l'intelligenza artificiale per trasformare input in linguaggio naturale in query SQL eseguibili. Questo processo consente agli utenti di interrogare database senza la necessità di conoscere il linguaggio SQL, rendendo l'accesso ai dati più intuitivo e accessibile.

## 2 - Scopo delle Domande in ThothSL

Nell'applicazione ThothSL, le Domande sono gli input forniti dagli utenti sotto forma di frasi o quesiti in linguaggio naturale. L'obiettivo principale è interpretare queste domande per generare automaticamente query SQL che estraggano le informazioni richieste dal database. Ad esempio, una domanda come "Qual è il fatturato medio per cliente?" viene analizzata e tradotta in una query SQL corrispondente che calcola tale valore.

Il sistema utilizza agenti AI per analizzare il testo della domanda, estrarre parole chiave, recuperare contesti rilevanti e suggerimenti (hints) da un database vettoriale, e infine generare una query SQL valida. Questo approccio permette di gestire una vasta gamma di domande, anche complesse, garantendo risultati accurati e pertinenti.

## 3 - Informazioni Associate alle Domande

Ogni Domanda memorizzata nel sistema è accompagnata da informazioni specifiche che ne facilitano l'elaborazione e la comprensione:

- **Testo della Domanda**: Il testo originale inserito dall'utente in linguaggio naturale.
- **Query SQL Generata**: Il codice SQL prodotto dall'applicazione in risposta alla domanda, che può essere eseguito sul database per ottenere i dati richiesti.
- **Suggerimento (Hint)**: Un testo opzionale che fornisce contesto o indicazioni aggiuntive relative alla domanda, utile per migliorare l'accuratezza della generazione SQL.

Queste informazioni sono memorizzate come parte di un oggetto `SqlDocument` nel database vettoriale, consentendo un accesso rapido e organizzato.

## 4 - Caricamento delle Domande nel Backend di Thoth

Il backend di Thoth, che condivide il database vettoriale con ThothSL, gestisce il caricamento e la memorizzazione delle Domande attraverso un sistema strutturato:

- **Database Vettoriale**: Le Domande vengono caricate in un database vettoriale, che permette di associarle a vettori per la ricerca semantica. Questo facilita il recupero di esempi simili o suggerimenti durante la generazione di SQL.
- **Processo di Caricamento**: Gli utenti possono caricare Domande nel sistema tramite un'interfaccia dedicata. Il processo è supportato da funzioni specifiche che leggono i dati (ad esempio da file CSV) e li inseriscono nel database vettoriale, aggiornando i metadati del workspace per tenere traccia dell'ultima operazione.
- **Gestione delle Domande**: Il backend offre funzionalità per creare, modificare ed eliminare Domande, garantendo che il database vettoriale rimanga aggiornato e coerente con le esigenze degli utenti.

## 5 - Interazione con l'Interfaccia Utente (UI) di Thoth

L'interfaccia utente di Thoth è progettata per rendere semplice e intuitiva la gestione delle Domande:

- **Visualizzazione delle Domande**: Una sezione dedicata dell'interfaccia (`questions.html`) mostra l'elenco delle Domande memorizzate nel database vettoriale. Gli utenti possono visualizzare il testo della domanda, la query SQL associata e eventuali suggerimenti correlati.
- **Caricamento e Gestione**: L'interfaccia fornisce pulsanti e moduli per caricare nuove Domande, modificarne di esistenti o eliminarle. Ad esempio, un pulsante "Carica Domande" avvia il processo di upload, mentre moduli specifici permettono di inserire o aggiornare i dettagli di una singola Domanda.
- **Feedback Utente**: Durante le operazioni di caricamento o gestione, l'interfaccia fornisce feedback immediato, come messaggi di conferma o avvisi di errore, per assicurare che gli utenti siano sempre informati sullo stato delle loro azioni.

In sintesi, le Domande sono il punto di partenza per l'interazione tra l'utente e il sistema Thoth, trasformando richieste in linguaggio naturale in query SQL eseguibili grazie a un'infrastruttura avanzata di AI e database vettoriali. La loro gestione attraverso il backend e l'interfaccia utente garantisce un'esperienza fluida e funzionale.
