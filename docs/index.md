# Benvenuti in Thoth

Thoth e un'applicazione che permette l'estrazione di dati dai database relazionali utilizzando l'AI per produrre ed eseguire le istruzioni SQL necessarie.
Le applicazioni di questo tipo sono denominate `Text-to-SQL`

Ciò che caratterizza Thoth è:

- la disponibilità di una **interfaccia utente** molto semplice da usare e allo stesso tempo molto adattabile tramite parametri di configurazione
- la possibilità di indicare, tra i parametri:

    1. gli **utenti** abilitati all'uso dell'applicazione e le loro autorizzazioni
    2. i **database da interrogare** e i database vettoriali da associare per la conservazione dei matadati necessari per facilitare la trasformazione della richiesta in SQL
    3. i **modelli LLM** da utilizzare

- l'utilizzo di un'applicazione specifica di backend per la gestione:

    1. dei parametri di configurazione
    2. dei metadati del database da interrogare (tables, columns, relationships)
    3. delle descrizioni di tabelle e colonne, che possono essere generate tramite AI
    4. del preprocessing del database da esaminare per creare hash e vettori che facilitino il processo di generazione del SQL
    5. di `hints` che possano chiarire termini complessi o suggerire interpretazioni non intuitive e, soprattutto, non facilmente ricavabili dai nomi dei campi e dalle descrizioni di campi e tabelle.

- l'utilizzo di un database vettoriale per la conservazione di `hints` e di associazioni 'question/SQL' che può essere alimentato dagli utenti autorizzati 
- l'ampio uso di LLM, anche di piccola o media dimensione, per effettuare le varie fasi del workflow di generazione del SQL da eseguire
- la possibilità di adattare il processo di generazione alla complessità e alla dimensione del roprio schema di database.

Per utilizzare al meglio Thoth si deve:
- installare correttamente l'applicazione, preferibilmente sotto Docker, seguendo le [istruzioni di installazione](1-install/1.1-sources_cloning.md)

## Cos'e il Text-to-SQL
Il text-to-SQL e una tecnologia che permette di convertire automaticamente domande o richieste espresse in linguaggio naturale in query SQL strutturate. Questa tecnologia rappresenta un ponte tra l'utente finale e i database, eliminando la necessità di conoscere la sintassi SQL per interrogare i dati.

### Caratteristiche fondamentali:
1. Comprensione del linguaggio naturale: L'applicazione deve essere in grado di interpretare domande formulate nella lingua utilizzata dall'utente e comprenderne l'intento
2. Mappatura semantica: Deve collegare i concetti espressi nel linguaggio naturale agli elementi del database (tabelle, colonne, relazioni)
3. Generazione di SQL valido: Deve produrre query SQL sintatticamente corrette e semanticamente appropriate
4. Gestione del contesto: Deve comprendere il dominio specifico e la struttura del database per fornire risultati accurati

### Vantaggi principali:
- Permette a utenti non tecnici di interrogare database complessi anche quando non si hanno a disposizione strumenti di Business Intelligence predisposti allo scopo
- Riduce il tempo necessario per formulare query complesse
- Minimizza gli errori di sintassi SQL
- Rende l'analisi dei dati accessibile a un pubblico piu ampio

## Le principali difficoltà del Text-to-SQL

Sebbene il `Text-to-SQL` sia una tecnologia promettente, per ottenere risultati soddisfacenti è necessario superare diverse difficoltà.

### 1. Problemi di contesto e documentazione dello schema

**Documentazione insufficiente dello schema:**
- **Nomi dei campi poco descrittivi**: Spesso i database utilizzano abbreviazioni, codici o convenzioni di naming che non sono immediatamente comprensibili (es. "cd_cli" invece di "codice_cliente")
- **Lingua non inglese**: Molti database sono progettati con nomi di tabelle e campi in lingue diverse dall'inglese, creando difficoltà per i modelli AI addestrati principalmente su testi inglesi
- **Mancanza di commenti**: Assenza di documentazione tecnica che spieghi il significato e l'uso dei vari campi
- **Terminologia specifica del dominio**: Presenza di "jargon" aziendale o settoriale che richiede conoscenze specifiche del contesto di business
- **Relazioni implicite**: Mancanza di Foreign Keys esplicitamente definite, rendendo difficile comprendere le relazioni tra le tabelle

### 2. Limitazioni nella comprensione dei dati

**Mancanza di informazioni sui valori:**
- Assenza di esempi dei valori contenuti nei campi, che potrebbero aiutare a comprendere meglio il significato e l'uso delle colonne
- Difficoltà nel comprendere i domini di valori possibili e le loro relazioni semantiche

### 3. Problemi di scalabilità

**Dimensioni dello schema:**
- Database con molte tabelle e colonne possono "confondere" i modelli AI più piccoli
- Difficoltà nel mantenere il contesto quando lo schema è molto ampio
- Necessità di strategie di selezione intelligente delle tabelle rilevanti per una specifica query

### 4. Sfide tecniche nella mappatura semantica

**Ambiguità linguistica:**
- Una stessa richiesta in linguaggio naturale può essere interpretata in modi diversi
- Difficoltà nel disambiguare termini che potrebbero riferirsi a più entità del database

**Complessità delle query:**
- Necessità di tradurre richieste complesse che coinvolgono aggregazioni, join multipli, subquery
- Gestione di logiche condizionali articolate espresse in linguaggio naturale

### 5. Gestione del dominio specifico

**Conoscenza del business:**
- Ogni settore ha le sue specificità e convenzioni che devono essere comprese dal sistema
- Necessità di adattare l'interpretazione al contesto aziendale specifico

**Evoluzione del sistema:**
- I database cambiano nel tempo, richiedendo aggiornamenti continui della mappatura semantica
- Necessità di mantenere coerenza tra le interpretazioni nel tempo
  


