# Installazione sotto Docker
L'installazione più semplice è quella che può essere effettuata utilizzando Docker. 

!!! note "Installazione di Docker"

    L'installazione e la configurazione di Docker sono attività al di fuori dello scope di questo documento.
    Eventualmente seguire li istruzioni per [Installare Docker sul vostro sistema](https://docs.docker.com/engine/install/)

Dando per scontato che Docker sia installato e funzionante, queste sono le azioni da eseguire per installare Thoth sotto Docker

## 1 - Procedura di Setup di Thoth (backend)
Il setup di Thoth sotto Docker prevede le seguenti azioni:

### 1.1 - Impostazione del file _env
Per prima cosa è necessario creare il file _env da inserire nella directory Thoth. Per farlo si deve:  

    1. copiare il file _env.template in _env
    2. aprire con un editor qualunque il file _env e riempire i placeholder delle API key degli LLM che intendete utilizzare
    3. inserire la chiavi SECRET_KEY e DJANGO_API_KEY appena generate negli appositi placeholder
    
### 1.2 - Esecuzione del docker-compose up

```bash
docker-compose up --build -d
```

Una volta eseguito il docker-compose up verificare che i processi siano correttamente funzionanti con.

```bash
docker ps
```

Deve comparire una lista come la seguente:
![Lista containers](../assets/ps-e.png)

## 1.3 - Setup del backend
### 1.3.1 - Creazione di un superuser
Il primo comando crea un utente superuser da utilizzare per accedere al backend.
```bash
python manage.py createsuperuser
```

Rispondere alle domande poste da Thoth (in quanto applicazione DJango) e creare un utente superuser.

###  1.3.2 - Setup iniziale
Procedere quindi a caricare un setup base completo e consistente da usare come setup minimo iniziale.
```bash
python manage.py import_groups
python manage.py import_users
python manage.py import_all_csv --docker
```

Con questi due comandi si creano tre gruppi (Admin, Editor, User) e due utenti (marco e maria).
Inoltre si crea un setup completo con delle configurazioni base funzionanti, associate agli utenti marco e maria, che possono essere usate come template.

### 1.3.3 Verifica del funzionamento dell'applicazione sotto Docker
A questo punto è possibilile aprire **http://localhost:8040**, fare login con l'utente definito come superuser e ritrovarsi di fronte a questa form:
![screenshot-01](../assets/screenshot-01.png)

La componente di backend di Thoth è "up and running" su Docker e si può passare al completamento del suo setup, che viene documentato nell'apposita pagina


## 1.3.4 Verifica del funzionamento dell'applicazione di backend in locale
A questo punto è possibilile aprire **http://localhost:8040**, fare login con l'utente definito come superuser e ritrovarsi di fronte a questa form:
![screenshot-01](../assets/screenshot-01.png)

La componente di backend di Thoth è "up and running" sotto Docker e si può passare al completamento del suo setup, che viene documentato nell'apposita pagina.

# 2 - Installazione dell'applicazione di frontend, ThothSL
Una installato il backend, si può installare il frontend, dal nome ThothSL (SL sta per Streamlit che è il tool usato per gestire la user interface). Entrare quindi nel progetto ThothSL e procedere come segue

## 2.1 - Impostazione del file _env
Copiare _env.template in _env e compilare i placeholder necessari. 
Devono essere necessariamente inserite le key di Django DJANGO_API_KEY, di Logfire e di almeno un provider LLM

## 2.2 - Esecuzione del docker-compose up

```bash
docker-compose up --build -d
```

Compare una form di login all'indirizzo http://localhost:8501. Per ora interrompere qui l'installazione del frontend in attesa di completare il setup del frontend, cosa che viene documentata [nell'apposita sezione di questo documento](/completamento_setup_backend/).
