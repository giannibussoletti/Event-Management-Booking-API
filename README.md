**English** | [Italiano](README.it.md)

# Event Management & Booking API

RESTful back-end built with **Spring Boot** for complete management of events and bookings. The platform distinguishes between two user types, **User** and **Organizer**, each with dedicated permissions, and covers the entire lifecycle of an event: from creation to booking, through to cancellation.

---

## Table of Contents

- [Main Features](#-main-features)
- [Tech Stack](#-tech-stack)
- [Requirements](#-requirements)
- [Installation and Configuration](#-installation-and-configuration)
- [Running the Application](#-running-the-application)
- [Testing the APIs with Postman](#-testing-the-apis-with-postman)
- [Endpoint Documentation](#-endpoint-documentation)

---

## 🎯 Main Features

The system implements **Role-Based Access Control**, with authentication and authorization handled through **JWT**.

### Organizer

- Create new events
- Edit and delete the events they authored
- Access a personal area listing their own events

### User

- Browse available events
- Book a seat at an event
- Cancel their own booking
- View their booking history

---

## 🛠 Tech Stack

| Area | Technology |
|---|---|
| Language | Java 25 |
| Framework | Spring Boot (Spring Web, Spring Data JPA, Spring Security) |
| Database | PostgreSQL |
| Security | JSON Web Token (JWT) |
| Build tool | Maven |

---

## 📋 Requirements

To run the project locally you will need:

- **JDK 25**
- **PostgreSQL**, optionally with a GUI client (pgAdmin or DBeaver)
- **A Java IDE** (IntelliJ IDEA, Eclipse or VS Code)
- **Postman** or an equivalent REST client to test the APIs

---

## 🔧 Installation and Configuration

### 1. Database Setup

1. Create an empty database in PostgreSQL (for example `gestione_eventi`).
2. There's no need to create the tables manually: the schema is generated automatically by Hibernate on first startup, thanks to the `ddl-auto=update` property.

### 2. Environment Setup

Sensitive credentials are managed through an external file excluded from version control.

1. In the project's root directory (the same one containing `pom.xml`), create a file named `env.properties`.
2. Add the following configuration, replacing the values with those of your environment:

```properties
PORT=8080
DB_URL=jdbc:postgresql://localhost:5432/gestione_eventi
DB_USERNAME=postgres
DB_PASSWORD=your_postgres_password
JWT_SECRET=your_long_and_complex_secret_key
```

> **Note:** the `JWT_SECRET` value must be a long, hard-to-guess string. The `env.properties` file must never be shared or committed to the repository.

---

## 🚀 Running the Application

The application can be started in two ways:

- **From the IDE:** open the project, wait for the Maven dependencies to download and run the main class annotated with `@SpringBootApplication`.
- **From the terminal:**

  ```bash
  ./mvnw spring-boot:run
  ```

A successful startup is confirmed in the console, which shows the application listening on the port defined in `env.properties` (for example `8080`).

---

## 🧪 Testing the APIs with Postman

Since this is a back-end without a graphical interface, its features are accessed through a REST client.

1. **Sign up** — `POST http://localhost:8080/auth/signup`
   The JSON body contains the data needed to create a new User or Organizer.
2. **Log in** — `POST http://localhost:8080/auth/login`
   The body contains the registered credentials; the response returns the **JWT token**.
3. **Authenticating protected requests**
   In Postman's **Authorization** tab, select the **Bearer Token** type and paste the token obtained at login.

---

## 📡 Endpoint Documentation

### Authentication (public)

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/auth/signup` | Register a new profile |
| `POST` | `/auth/login` | Authenticate and generate the JWT token |

### Events (protected)

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/events` | List all available events |
| `DELETE` | `/events/me/d/{eventId}` | Delete an event (restricted to the organizer who created it) |

### Bookings (protected)

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/bookings` | Retrieve booking information |
| `POST` | `/bookings/{eventId}` | Create a booking for an event |
| `PATCH` | `/bookings/{bookingId}` | Cancel or update an existing booking |

### Personal Area (protected)

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/users/me/events` | Events created by the authenticated organizer |
| `GET` | `/users/me/bookings` | Booking history of the authenticated user |
