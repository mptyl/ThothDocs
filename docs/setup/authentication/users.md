# Gestione Utenti

La gestione degli utenti in Thoth si basa sul sistema di autenticazione standard di Django, che fornisce un solido framework per la gestione di utenti, gruppi e permessi. Questa sezione descrive come gli utenti sono gestiti all'interno di Thoth e quali informazioni sono associate a ciascun utente.

## 1 - Il Concetto di Utente

Nel sistema di autenticazione di Django, un "User" (utente) rappresenta un individuo che può accedere e interagire con l'applicazione. Ogni utente ha le proprie credenziali (username e password) e un insieme di permessi che definiscono quali azioni può compiere all'interno del sistema.

Thoth utilizza il modello `User` standard di Django senza estensioni dirette. Ciò significa che le informazioni di base gestite per ogni utente sono quelle predefinite da Django.

## 2 - Informazioni Gestite nel Form Utente

Quando si crea o si modifica un utente dall'interfaccia di amministrazione di Thoth, vengono gestite le seguenti sezioni di informazioni:

### 2.1 - Dati Personali
- **Username**: Il nome univoco utilizzato per l'accesso.
- **Password**: La password per l'autenticazione. Per motivi di sicurezza, la password non viene mai mostrata in chiaro.
- **First name**: Il nome di battesimo dell'utente.
- **Last name**: Il cognome dell'utente.
- **Email address**: L'indirizzo email dell'utente.

### 2.2 - Stato e Permessi
- **Active**: Un flag che indica se l'utente è attivo. Se disattivato, l'utente non potrà effettuare il login.
- **Staff status**: Un flag che indica se l'utente può accedere all'interfaccia di amministrazione.
- **Superuser status**: Un flag che, se attivo, conferisce all'utente tutti i permessi del sistema, bypassando qualsiasi controllo specifico.

### 2.3 - Gruppi e Permessi Individuali
- **Groups**: In questa sezione è possibile assegnare l'utente a uno o più gruppi. Le autorizzazioni in Thoth sono principalmente gestite a livello di gruppo.
- **User permissions**: È possibile assegnare permessi specifici direttamente a un utente, anche se la pratica consigliata è quella di gestire i permessi tramite i gruppi.

## 3 - Il Ruolo dei Gruppi e dei Profili di Gruppo

La vera personalizzazione della gestione degli utenti in Thoth avviene a livello di **Gruppo**. Mentre il modello `User` è standard, ogni `Group` in Thoth è associato a un **Profilo di Gruppo** (`GroupProfile`) che definisce in dettaglio le funzionalità e le visualizzazioni a cui gli utenti di quel gruppo avranno accesso.

Quando un utente viene assegnato a un gruppo, eredita tutte le impostazioni definite nel profilo di quel gruppo. Questo permette di configurare in modo centralizzato e coerente l'esperienza utente per intere categorie di utenti.

Per i dettagli su come configurare i gruppi e i loro profili, si rimanda alla documentazione specifica:
- [Gestione Gruppi](./groups.md)
- [Profili di Gruppo](./group_profiles.md)
