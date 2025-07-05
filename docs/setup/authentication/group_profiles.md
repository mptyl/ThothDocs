# Group Profiles

## 1 - Cos'è un Group Profile

In Thoth, il **Group Profile** è un'estensione del modello standard di `Group` fornito da Django. Ogni volta che viene creato un nuovo gruppo di utenti, il sistema associa automaticamente un profilo a quel gruppo.

Lo scopo principale del `GroupProfile` è quello di fornire un controllo granulare sulle funzionalità e sulle informazioni visualizzate dagli utenti appartenenti a un determinato gruppo. Attraverso la configurazione del profilo, un amministratore può personalizzare l'esperienza utente, decidendo quali elementi dell'interfaccia e quali dettagli dei processi interni devono essere visibili.

## 2 - Campi del Group Profile

Il form di amministrazione per un `GroupProfile` espone una serie di opzioni booleane (Sì/No) che permettono di configurare in dettaglio i permessi e la visibilità per gli utenti del gruppo associato.

### 2.1 - Show SQL

*   **Nome del campo**: `show_sql`
*   **Scopo**: Se attivato, gli utenti appartenenti a questo gruppo potranno visualizzare la query SQL finale generata dal sistema in risposta alle loro richieste. È utile per utenti esperti o per scopi di debug.

### 2.2 - Show keywords

*   **Nome del campo**: `show_keywords`
*   **Scopo**: Controlla la visibilità della sezione in cui vengono mostrate le parole chiave estratte dalla richiesta dell'utente. Questa opzione è generalmente utile per comprendere come il sistema interpreta il linguaggio naturale.

### 2.3 - Show hints

*   **Nome del campo**: `show_hints`
*   **Scopo**: Se abilitato, gli utenti visualizzeranno i "suggerimenti" (hints) che il sistema fornisce per aiutarli a formulare meglio le loro domande o a esplorare i dati.

### 2.4 - Show process info

*   **Nome del campo**: `show_process_info`
*   **Scopo**: Permette di mostrare o nascondere le informazioni dettagliate sul processo di elaborazione interno di Thoth. Se attivato, l'utente può vedere i vari passaggi che il sistema compie per arrivare al risultato finale. È un'opzione consigliata per utenti avanzati o amministratori.

### 2.5 - Show SQL generation

*   **Nome del campo**: `show_sql_generation`
*   **Scopo**: Controlla la visibilità del processo passo-passo che porta alla generazione della query SQL. Se attivo, l'utente può seguire le fasi intermedie, come la selezione delle colonne e delle tabelle.

### 2.6 - Explain generated query

*   **Nome del campo**: `explain_generated_query`
*   **Scopo**: Se questa opzione è attiva, il sistema utilizzerà un modello di intelligenza artificiale per fornire una spiegazione in linguaggio naturale della query SQL generata. È molto utile per gli utenti che non hanno familiarità con il linguaggio SQL ma desiderano comprendere la logica dietro la risposta.

### 2.7 - Treat empty result as an error

*   **Nome del campo**: `treat_empty_result_as_error`
*   **Scopo**: Stabilisce come il sistema deve comportarsi quando una query SQL eseguita correttamente non produce alcun risultato. Se disattivato (impostazione predefinita), un risultato vuoto è considerato una risposta valida. Se attivato, viene trattato come un errore, notificando all'utente che la sua richiesta non ha trovato corrispondenze.

## 3 - Gestione dei Group Profiles

La gestione dei profili dei gruppi è integrata direttamente nell'interfaccia di amministrazione di Django, rendendo il processo semplice e centralizzato.

Per modificare un `GroupProfile`, segui questi passaggi:

1.  Accedi al pannello di amministrazione di Django.
2.  Nella sezione "Authentication and Authorization", fai clic su "Groups".
3.  Seleziona il gruppo che desideri modificare.
4.  Nella pagina di modifica del gruppo, troverai la sezione "Group Profile Settings" dove potrai configurare tutte le opzioni descritte in questo documento.
5.  Dopo aver apportato le modifiche, salva il form.

Le nuove impostazioni verranno applicate immediatamente a tutti gli utenti che appartengono a quel gruppo.
