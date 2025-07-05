# Gestione dei Gruppi

Nel sistema di autenticazione di Django, un gruppo è un modo per categorizzare gli utenti e assegnare loro permessi in modo collettivo. Invece di assegnare permessi a ogni singolo utente, è possibile assegnarli a un gruppo, e tutti gli utenti in quel gruppo erediteranno tali permessi.

In Thoth, il concetto di gruppo è stato esteso per offrire un controllo più granulare sull'esperienza utente. Questo è stato realizzato attraverso un "Profilo di Gruppo" (`GroupProfile`) che è associato a ogni gruppo standard di Django.

## 1 - Il Profilo del Gruppo

Quando si crea o si modifica un gruppo nell'interfaccia di amministrazione di Thoth, oltre al nome e ai permessi standard, è possibile configurare una serie di impostazioni aggiuntive che definiscono come gli utenti di quel gruppo interagiranno con il sistema.

Queste impostazioni sono raggruppate nella sezione "Group Profile Settings".

### 1.1 - Impostazioni di Visualizzazione

Le seguenti opzioni controllano quali informazioni vengono mostrate agli utenti del gruppo durante le loro interazioni con l'assistente AI.

- **Show SQL**: Se attivato, gli utenti potranno vedere le query SQL generate dall'AI. Questo è utile per utenti tecnici che vogliono verificare o comprendere la logica dietro le risposte.
- **Show keywords**: Se attivato, verranno mostrate le parole chiave estratte dalla richiesta dell'utente. Questo aiuta a capire come l'AI ha interpretato la domanda.
- **Show hints**: Se attivato, gli utenti visualizzeranno i "suggerimenti" (hints) che il sistema ha trovato pertinenti alla loro richiesta. I suggerimenti sono informazioni pre-caricate che guidano l'AI nella generazione di risposte più accurate.
- **Show process info**: Se attivato, verranno mostrate informazioni dettagliate sul processo di elaborazione della richiesta, come i vari passaggi eseguiti dall'AI.
- **Show SQL generation**: Se attivato, l'utente potrà vedere il processo di generazione della query SQL, inclusi i passaggi intermedi e le decisioni prese dall'AI.
- **Explain generated query**: Se attivato, l'AI fornirà una spiegazione in linguaggio naturale della query SQL generata. Questo è particolarmente utile per utenti non tecnici per comprendere lo scopo della query.

### 1.2 - Gestione dei Risultati

Questa impostazione definisce come il sistema deve comportarsi in una situazione specifica.

- **Treat empty result set as an error**: Se attivato, un risultato di query che non produce righe (un "empty set") sarà trattato come un errore. Questo può essere utile in scenari in cui ci si aspetta sempre un risultato e un set vuoto indica un problema o una domanda mal posta. Se disattivato, un set di risultati vuoto è considerato una risposta valida.
