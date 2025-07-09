# Modelli AI di Base

I Modelli AI di Base (Basic AI Models) rappresentano le categorie generiche dei modelli di linguaggio (LLM) supportati da Thoth, raggruppati per fornitore. Ogni record definisce un provider di base, come OpenAI, Anthropic (Claude), Mistral, ecc.

Questa astrazione permette di organizzare e gestire centralmente i diversi tipi di LLM disponibili nel sistema, fungendo da base per la configurazione di modelli più specifici.

## 1 - Struttura del Modello

La tabella `BasicAiModel` contiene le informazioni fondamentali per identificare e descrivere una famiglia di modelli AI.

### 1.1 - Campi del Modello

Di seguito sono elencati i campi che definiscono un Modello AI di Base:

- **Name**: Un nome univoco che identifica la famiglia di modelli (es. "OpenAI", "Anthropic"). Questo campo è obbligatorio e deve essere unico.
- **Description**: Un campo di testo opzionale per fornire una descrizione dettagliata della famiglia di modelli o del provider.
- **Provider**: Specifica il fornitore del modello di linguaggio. Il valore viene selezionato da un elenco predefinito di scelte, che include opzioni come:
    - `OPENAI`
    - `ANTHROPIC` (Claude)
    - `CODESTRAL`
    - `DEEPSEEK`
    - `META` (LLama)
    - `MISTRAL`
    - `OLLAMA`
    - `OPENROUTER`
    - `GEMINI`
- **Created At**: La data e l'ora in cui il record è stato creato.
- **Updated At**: La data e l'ora dell'ultima modifica del record.

## 2 - Relazioni con Altri Modelli

Il `BasicAiModel` ha una relazione diretta e fondamentale con il modello `AiModel`.

### 2.1 - Relazione con AiModel

La relazione tra `BasicAiModel` e `AiModel` è di tipo **uno-a-molti**. Questo significa che:

- Ogni `BasicAiModel` può essere associato a uno o più `AiModel`.
- Ogni `AiModel` deve essere collegato a un solo `BasicAiModel`.

Ad esempio, un `BasicAiModel` con `name` "OpenAI" può avere più `AiModel` associati, come "gpt-4", "gpt-3.5-turbo", ecc. Questa struttura permette di raggruppare tutte le varianti di un provider sotto un'unica categoria, semplificando la gestione e la selezione dei modelli all'interno dei Workspace.

## 3 - Gestione dall'Interfaccia di Amministrazione

La gestione dei Modelli AI di Base avviene tramite il pannello di amministrazione di Django, che offre un'interfaccia intuitiva per eseguire tutte le operazioni necessarie.

### 3.1 - Visualizzazione e Ricerca

Nella sezione di amministrazione, la lista dei `BasicAiModel` mostra le seguenti colonne:

- **Name**: Il nome univoco del modello base.
- **Description**: La descrizione fornita.
- **Provider**: Il fornitore del modello.

È possibile utilizzare la barra di ricerca per trovare un modello specifico cercando per `name` o `provider`. Inoltre, sono disponibili filtri per restringere la visualizzazione in base al nome, alla descrizione o al provider.

### 3.2 - Creazione e Modifica

Per creare un nuovo `BasicAiModel`, è sufficiente fare clic sul pulsante "Add Basic AI Model" e compilare i campi del form. Durante la modifica, tutti i campi possono essere aggiornati secondo necessità.

### 3.3 - Azioni Disponibili

Dalla lista dei `BasicAiModel`, è possibile eseguire le seguenti azioni di gruppo su uno o più record selezionati:

- **Export selected items to CSV**: Esporta i modelli base selezionati in un file CSV.
- **Import from CSV**: Importa nuovi modelli base o aggiorna quelli esistenti partendo da un file CSV. Questa funzionalità è utile per la configurazione massiva del sistema.
