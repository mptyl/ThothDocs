# Agents

Gli agenti in Thoth sono entità specializzate che utilizzano i modelli di intelligenza artificiale (AI Models) per eseguire compiti specifici all'interno del flusso di lavoro di generazione SQL. Ogni agente è configurato per svolgere un ruolo preciso, come l'estrazione di parole chiave, la selezione di colonne o la generazione di codice SQL.

## 1. - Tipi di Agenti

Thoth prevede diversi tipi di agenti, ognuno progettato per una fase specifica del processo. I tipi di agenti disponibili sono definiti nel sistema e possono essere selezionati durante la creazione o la modifica di un agente.

I tipi di agenti sono:

- **Generate SQL Basic**: Un agente di base per la generazione di query SQL.
- **Default**: Un tipo di agente generico.
- **Extract Keywords**: Estrae le parole chiave rilevanti dalla domanda dell'utente.
- **Select Columns**: Seleziona le colonne del database più pertinenti per la domanda.
- **Sql - Basic**: Genera query SQL di base.
- **Sql - Advanced**: Genera query SQL più complesse.
- **Sql - Expert**: Genera query SQL a livello di esperto.
- **Sql - Titan**: Genera le query SQL più complesse, utilizzate nei workflow più avanzati.
- **Test Generator**: Genera test per verificare la correttezza delle query SQL.
- **Test Executor**: Esegue i test generati.
- **Ask For Human Help**: Gestisce i casi in cui è necessario l'intervento umano.

## 2. - Gestione degli Agenti

La gestione degli agenti avviene tramite l'interfaccia di amministrazione di Django. Da qui è possibile creare, modificare ed eliminare gli agenti.

### 2.1 - Creazione e Modifica di un Agente

Per creare un nuovo agente o modificarne uno esistente, è necessario compilare i seguenti campi:

- **Name**: Un nome univoco per identificare l'agente.
- **Agent Type**: Il tipo di agente, selezionato da un elenco a discesa (vedi sezione 1).
- **AI Model**: Il modello di intelligenza artificiale che l'agente utilizzerà per eseguire i suoi compiti. Questo è un collegamento a un'istanza di `AiModel` preconfigurata.
- **Temperature**: Un valore decimale che controlla la casualità dell'output del modello AI. Valori più alti rendono l'output più vario, mentre valori più bassi lo rendono più deterministico.
- **Top_p**: Un parametro alternativo alla temperatura per controllare la casualità.
- **Max Tokens**: Il numero massimo di token (parole o pezzi di parole) che il modello AI può generare in una singola risposta.
- **Timeout**: Il tempo massimo, in secondi, che l'agente attenderà per una risposta dal modello AI.
- **System Prompt**: Un'istruzione o un contesto fornito al modello AI per guidare il suo comportamento. Questo prompt viene utilizzato per definire il ruolo e gli obiettivi dell'agente.
- **User Prompt**: Il prompt che simula l'input dell'utente, utilizzato in combinazione con il System Prompt.
- **Retries**: Il numero di tentativi che l'agente effettuerà in caso di fallimento.

### 2.2 - Visualizzazione degli Agenti

La pagina di elenco degli agenti nell'interfaccia di amministrazione mostra una panoramica di tutti gli agenti configurati, con colonne per:

- **Name**: Il nome dell'agente.
- **Agent Type**: Il tipo di agente.
- **AI Model**: Il nome del modello AI associato.
- **Temperature**: La temperatura configurata.
- **Top_p**: Il valore di top_p configurato.
- **Max Tokens**: Il numero massimo di token.
- **Timeout**: Il timeout configurato.

È possibile filtrare gli agenti per tipo e per modello AI di base, e cercarli per nome.

## 3. - Relazioni con altri Modelli

### 3.1 - Relazione con AiModel

Ogni agente deve essere associato a un `AiModel`. Questa relazione è fondamentale, poiché l'agente delega al modello AI l'esecuzione dei compiti di elaborazione del linguaggio naturale. La configurazione dell'agente (come `temperature` e `max_tokens`) può sovrascrivere le impostazioni predefinite del modello AI associato, consentendo una personalizzazione fine per ogni specifico compito.

### 3.2 - Relazione con Workspace

Gli agenti sono utilizzati all'interno dei `Workspace`. Un workspace definisce un ambiente di lavoro completo, che include un database SQL e una serie di agenti configurati per interagire con esso.

Nel modello `Workspace`, ci sono più campi che collegano a diversi agenti, ognuno con un ruolo specifico nel flusso di lavoro. Ad esempio, un workspace ha campi come:

- `kw_sel_agent_1` e `kw_sel_agent_2` per l'estrazione di parole chiave.
- `sel_columns_agent_1` e `sel_columns_agent_2` per la selezione delle colonne.
- `sql_basic_agent`, `sql_advanced_agent`, `sql_expert_agent`, e `sql_titan_agent_1` / `sql_titan_agent_2` per i diversi livelli di generazione SQL.
- `test_gen_agent_1` e `test_gen_agent_2` per la generazione di test.
- `test_exec_agent` per l'esecuzione dei test.
- `ask_human_help_agent` per richiedere assistenza.

Questa struttura permette di creare flussi di lavoro complessi e personalizzati, in cui diversi agenti collaborano per trasformare una domanda in linguaggio naturale in una query SQL valida e ottimizzata.
