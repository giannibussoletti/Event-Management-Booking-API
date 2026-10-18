[English](README.md) | **Italiano**

# Event Management & Booking API

Backend RESTful sviluppato con **Spring Boot** per la gestione completa di eventi e prenotazioni. La piattaforma distingue due tipologie di utenti, **Utente** e **Organizzatore**, ciascuna con permessi dedicati, e copre l'intero ciclo di vita di un evento: dalla creazione alla prenotazione, fino all'annullamento.

---

## Indice

- [Funzionalità principali](#-funzionalità-principali)
- [Stack tecnologico](#-stack-tecnologico)
- [Requisiti](#-requisiti)
- [Installazione e configurazione](#-installazione-e-configurazione)
- [Avvio dell'applicazione](#-avvio-dellapplicazione)
- [Test delle API con Postman](#-test-delle-api-con-postman)
- [Documentazione degli endpoint](#-documentazione-degli-endpoint)

---

## 🎯 Funzionalità principali

Il sistema implementa un controllo degli accessi basato sui ruoli (**Role-Based Access Control**), con autenticazione e autorizzazione gestite tramite **JWT**.

### Organizzatore

- Creazione di nuovi eventi
- Modifica ed eliminazione degli eventi di cui è autore
- Consultazione di un'area personale con l'elenco dei propri eventi

### Utente

- Consultazione degli eventi disponibili
- Prenotazione di un posto a un evento
- Annullamento della propria prenotazione
- Consultazione dello storico delle proprie prenotazioni

---

## 🛠 Stack tecnologico

| Ambito | Tecnologia |
|---|---|
| Linguaggio | Java 25 |
| Framework | Spring Boot (Spring Web, Spring Data JPA, Spring Security) |
| Database | PostgreSQL |
| Sicurezza | JSON Web Token (JWT) |
| Build tool | Maven |

---

## 📋 Requisiti

Per l'esecuzione locale del progetto sono necessari:

- **JDK 25**
- **PostgreSQL**, con un client grafico opzionale (pgAdmin o DBeaver)
- **IDE Java** (IntelliJ IDEA, Eclipse o VS Code)
- **Postman** o un client REST equivalente per il test delle API

---

## 🔧 Installazione e configurazione

### 1. Configurazione del database

1. Creare in PostgreSQL un database vuoto (ad esempio `gestione_eventi`).
2. Non è necessario creare manualmente le tabelle: lo schema viene generato automaticamente da Hibernate al primo avvio, grazie alla proprietà `ddl-auto=update`.

### 2. Configurazione dell'ambiente

Le credenziali sensibili sono gestite tramite un file esterno, escluso dal versionamento.

1. Nella directory principale del progetto (la stessa in cui si trova `pom.xml`) creare un file denominato `env.properties`.
2. Inserire la seguente configurazione, sostituendo i valori con quelli del proprio ambiente:

```properties
PORT=8080
DB_URL=jdbc:postgresql://localhost:5432/gestione_eventi
DB_USERNAME=postgres
DB_PASSWORD=your_postgres_password
JWT_SECRET=your_long_and_complex_secret_key
```

> **Nota:** il valore di `JWT_SECRET` deve essere una stringa lunga e difficile da indovinare. Il file `env.properties` non deve mai essere condiviso né incluso nel repository.

---

## 🚀 Avvio dell'applicazione

L'applicazione può essere avviata in due modalità:

- **Da IDE:** aprire il progetto, attendere il download delle dipendenze Maven ed eseguire la classe principale annotata con `@SpringBootApplication`.
- **Da terminale:**

  ```bash
  ./mvnw spring-boot:run
  ```

Il corretto avvio è confermato dalla console, che indica l'applicazione in ascolto sulla porta definita in `env.properties` (ad esempio `8080`).

---

## 🧪 Test delle API con Postman

Trattandosi di un backend privo di interfaccia grafica, l'interazione con le funzionalità avviene tramite un client REST.

1. **Registrazione** — `POST http://localhost:8080/auth/signup`
   Il body, in formato JSON, contiene i dati per la creazione di un nuovo Utente o Organizzatore.
2. **Login** — `POST http://localhost:8080/auth/login`
   Il body contiene le credenziali registrate; la risposta restituisce il **token JWT**.
3. **Autenticazione delle richieste protette**
   Nella scheda **Authorization** di Postman selezionare il tipo **Bearer Token** e incollare il token ottenuto al login.

---

## 📡 Documentazione degli endpoint

### Autenticazione (pubblici)

| Metodo | Endpoint | Descrizione |
|---|---|---|
| `POST` | `/auth/signup` | Registrazione di un nuovo profilo |
| `POST` | `/auth/login` | Autenticazione e generazione del token JWT |

### Eventi (protetti)

| Metodo | Endpoint | Descrizione |
|---|---|---|
| `GET` | `/events` | Elenco di tutti gli eventi disponibili |
| `DELETE` | `/events/me/d/{eventId}` | Eliminazione di un evento (riservata all'organizzatore che lo ha creato) |

### Prenotazioni (protetti)

| Metodo | Endpoint | Descrizione |
|---|---|---|
| `GET` | `/bookings` | Recupero delle informazioni sulle prenotazioni |
| `POST` | `/bookings/{eventId}` | Creazione di una prenotazione per un evento |
| `PATCH` | `/bookings/{bookingId}` | Annullamento o aggiornamento di una prenotazione esistente |

### Area personale (protetti)

| Metodo | Endpoint | Descrizione |
|---|---|---|
| `GET` | `/users/me/events` | Eventi creati dall'organizzatore autenticato |
| `GET` | `/users/me/bookings` | Storico delle prenotazioni dell'utente autenticato |
