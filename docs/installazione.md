# Installazione di Thoth 

Thoth si può installare con Docker o in locale.

## 1 - Attività preliminare - clonazione dei progetti da GitHub

Prima di tutto, ovviamente, occorre clonare da GitHub i due repository. 

Da linea di comando posizionarsi dove si preferisce, creare una directory, ad esempio ThothApp, ed entrarci:
   ```bash
   mkdir ThothApp # o altro nome a vostra scelta
   cd ThothApp
   ```

Una volta entrati nella directory, che farà da root a tutta l'applicazione, clonare da GitHub i progetti di backend (Thoth) e di frontend (ThothSL)

   ```bash
   - git clone https://github.com/mptyl/Thoth.git 
   - git clone https://github.com/mptyl/ThothSL.git 
   ```

## 2 - Le API Keys
Prima di proseguire con l'installazione, è necessario dotarsi delle API Keys per:

* i servizi LLM che verranno utilizzerati all'interno del processo Thoth per generare un SQL a partire da un testo.
* il servizio di logging Logfire
* una corretta gestione del backend basato su Django

### 2.1 - I servizi di generazione tramite LLM
La generazione di un SQL a partire da un testo non può prescindere dall'utilizzo di un LLM.  
Thoth è predisposto per poter essere utilizzato dai seguenti fornitori di API:

    * OpenAI 
    * Gemini (di Google)
    * Anthropic 
    * Mistral
    * Codestral (di Mistral)
    * DeepSeek
    * Llama (di Meta)
    * Ollama
    * OpenRouter

Ogni fornitore di API, a sua volta, permette di usare diversi modelli, che andranno specificati nel setup dell'applicazione.

In fase di configurazione occorre stabilire quali provider si desidera utilizzare.  Per ognuno di essi, è necessario procurarsi un'API Key.
Se si utilizza Ollama in locale, ovviamente, non è necessaria alcuna API Key, ma solo indicare l'IP del server Ollama utilizzato. 

Per procurarsi l'API Key seguite le procedure previste dal provider.

!!! note "Assegnazione di una API Key"

    Le procedure di assegnazione di una API Key sono più o meno le stesse per ogni fornitore. 
    Creare delle API Key dal nome Thoth, o simile, per ogni fornitore che si desidera utilizzare. In seguita si vedrà dove dovranno essere utilizzate.

Se si usa OpenRouter, con una sola API Key si potrà accedere ad una lunga lista di servizi LLM, con dimensioni e costo molto vario (alcuni modelli sono resi disponibili, via API, addirittura gratuitamente). Andare sul sito di [OpenRouter](https://openrouter.ai/) per ulteriori dettagli.

I valori di default che sono impostati in Thoth prevedono che tutti gli Agent che svolgono le varie fasi del processo usino LLM forniti da OpenRouter. 

L'utente può ovviamente cambiare questo setup - si vedrà più avanti come - ma, se si vuole provare rapidamente Thoth, l'ideale è iniziare con l'acquistare  5 o 10 Dollari di servizi OpenRouter e lasciare le impostazioni di default che vengono impostate in fase di installazione. 

Dai nostri test, utilizzando la configurazione di default, ogni interrogazione ha un costo variante tra 1 e 2 centesimi di Dollaro, per cui, con una decina di Dollari, si possono fare circa 700 prove.

### 2.2 - Il servizio Logfire
[Logfire](https://pydantic.dev/logfire) è un servizio messo a disposizione da Pydantic per tracciare l'attività di applicazioni Python. Fino a 10 milioni di messaggi al mese è un servizio gratuito.  

Thoth produce un log delle sue attività contemporaneamente su console e su Logfire. Il log su Logfire persiste per circa 30 giorni e fornisce un elevato livello di dettaglio sulle operazioni registrate.

E' necessario quindi andare su Logfire e createvi una API Key gratuita

!!! note "Il logging di un LLM"

    Loggare le attività effettuate da un LLM è un'attività piuttosto complessa. Con Logfire si può facilmente verificare il testo del prompt ricevuto dal LLM, l'eventuale attivazione di Tools da parte degll'Agent in esecuzione e l'output generato, con abbondanza di dettagli.

### 2.3 - Il setup di Django

Il setup di Django richiede che vengano generate due API Key da impostare nel file di configurazione _env (per docker) o .env (per l'uso in locale)

#### La generazione della Django SECRET_KEY
Per generare una nuova chiave segreta Django si può utilizzare il generatore online https://djecrety.ir/

####La generazione della DJANGO_API_KEY
Per generare una chiave API sicura, da usare nella comunicazione tramite API Key del frontend col backend, utilizzare questo piccolo snippet Python:
```bash
python3 -c "import secrets; print(secrets.token_urlsafe(32))"
```

### La generazione dei file .env e _env
Una volta che si è in possesso delle chiavi degli LLM che si intendono utilizzare, di Logfire e di Django si può procedere a creare i file _env - per il deploy sotto Docker - o .env per il deploy il locale.

## Installazione sotto Docker
L'installazione più semplice è quella che può essere effettuata utilizzando Docker. 
L'installazione e la configurazione di Docker sono al di fuori dello scope di questo documento.
Dando per scontato che Docker sia installato e funzionante, vediamo le azioni da eseguire per installare Thoth sotto Docker

### Procedura di Setup di Thoth (backend)

#### Impostazione del file _env
Per prima cosa è necessario preparare il file _env da inserire nella directory Thoth. Per farlo si deve:  

    - copiate il file _env.template in _env
    - aprite con un editor qualunque il file _env e riempite i placeholder delle API key degli LLM che intendete utilizzare
    - compilate la chiavi SECRET_KEY e DJANGO_API_KEY appena generate negli appositi placeholder
    
#### Esecuzione del docker-compose up

```bash
docker-compose up --build -d
```

Una volta eseguito il docker-compose up verificare che i processi siano correttamente funzionanti.

```bash
docker ps
```
