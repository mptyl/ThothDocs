# AI Models

In Thoth, la configurazione dei modelli di intelligenza artificiale (LLM) è un passaggio cruciale che determina le capacità del sistema di comprendere le richieste in linguaggio naturale e generare query SQL. La gestione dei modelli è suddivisa in due entità principali: **Basic AI Models** e **AI Models**. Questa struttura offre flessibilità e organizzazione.

## 1 - Basic AI Models

Un **Basic AI Model** rappresenta una famiglia o un fornitore di modelli LLM. Ad esempio, "Claude 3" o "GPT-4" possono essere configurati come modelli di base. Questa entità raggruppa le configurazioni specifiche sotto un unico ombrello.

### 1.1 - Campi del Modello

Il form di amministrazione per i Basic AI Models contiene i seguenti campi:

*   **Name**: Un nome univoco che identifica la famiglia di modelli (es. `Claude 3`).
*   **Description**: Un campo di testo per una descrizione dettagliata del modello o del fornitore.
*   **Provider**: Il fornitore del servizio LLM, selezionabile da una lista predefinita che include opzioni come `OpenAI`, `Anthropic`, `Mistral`, `Ollama`, ecc.

### 1.2 - Gestione nell'Admin

Dall'interfaccia di amministrazione di Django, è possibile:

*   Visualizzare la lista dei modelli di base con i loro nomi, descrizioni e provider.
*   Filtrare e cercare i modelli per nome o provider.
*   Esportare e importare la lista dei modelli in formato CSV.

## 2 - AI Models

Un **AI Model** è una configurazione specifica di un `BasicAiModel`. Permette di definire i parametri esatti per l'utilizzo di una particolare versione di un modello (es. "Claude 3 Haiku") e di associare le credenziali di accesso (API key).

### 2.1 - Relazione con Basic AI Model

Esiste una relazione **uno-a-molti** tra `BasicAiModel` e `AiModel`. Un singolo `BasicAiModel` (es. "Claude 3") può avere più `AiModel` associati, ciascuno con una configurazione differente (es. "Claude 3 Haiku", "Claude 3 Sonnet", "Claude 3 Opus").

### 2.2 - Campi del Modello

Il form di amministrazione per gli AI Models è suddiviso in due sezioni per una maggiore chiarezza.

#### 2.2.1 - Sezione Principale

Questa sezione contiene le informazioni identificative e di accesso del modello:

*   **Basic model**: Un menu a discesa per selezionare il `BasicAiModel` a cui questa configurazione appartiene.
*   **Specific model**: Il nome tecnico o l'identificatore specifico del modello fornito dal provider (es. `claude-3-haiku-20240307`).
*   **Name**: Un nome descrittivo e amichevole per questa configurazione (es. `Claude 3 Haiku`).
*   **API Key**: La chiave API per l'autenticazione con il servizio LLM. Per motivi di sicurezza, la chiave viene mascherata e mostra solo gli ultimi quattro caratteri. Per aggiornarla, è sufficiente inserire un nuovo valore; per cancellarla, lasciare il campo vuoto.
*   **Url**: Un campo opzionale per specificare un URL di endpoint, utile per modelli self-hosted o servizi che non utilizzano l'endpoint standard del provider.

#### 2.2.2 - Sezione "Advanced"

Questa sezione, collassabile di default, contiene i parametri tecnici che influenzano il comportamento del modello:

*   **Temperature allowed**: Un selettore booleano che abilita o disabilita la possibilità di sovrascrivere la temperatura a livello di agente.
*   **Temperature**: Un valore numerico (da 0.0 a 2.0) che controlla la "creatività" del modello. Valori più bassi (es. 0.2) producono risposte più deterministiche, mentre valori più alti (es. 0.8) generano output più vari e creativi.
*   **Top P**: Un valore numerico (da 0.0 a 1.0) che rappresenta un metodo alternativo alla temperatura per controllare la casualità. Il modello seleziona i token la cui somma di probabilità cumulativa è inferiore o uguale a questo valore.
*   **Max Tokens**: Il numero massimo di token (unità di testo) che il modello può generare in una singola risposta.
*   **Timeout**: Il tempo massimo, in secondi, che il sistema attenderà per ricevere una risposta dal modello prima di interrompere la richiesta.

### 2.3 - Gestione nell'Admin

L'interfaccia di amministrazione per gli AI Models offre le seguenti funzionalità:

*   **Visualizzazione in Lista**: La lista mostra il nome specifico, il modello base associato e il nome descrittivo, facilitando l'identificazione delle configurazioni.
*   **Filtri e Ricerca**: È possibile cercare i modelli per nome (base, specifico o descrittivo) e filtrare per modello base.
*   **Azioni**: Come per i modelli di base, è possibile esportare e importare le configurazioni in formato CSV.

Questa struttura a due livelli consente una gestione ordinata e scalabile dei modelli di intelligenza artificiale, separando la definizione generale del fornitore dalla configurazione specifica di ogni singolo modello utilizzato all'interno di Thoth.
