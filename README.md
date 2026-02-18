# 🏦 Midas Core – Event-Driven Financial Transaction Processor

Midas Core is a Spring Boot–based backend service that processes financial transactions using an event-driven architecture.
It consumes transactions via Kafka, validates them against business rules, persists them in an H2 SQL database using JPA, integrates with an external Incentive API, and exposes a REST endpoint to query user balances.

## 🚀 Features
✅ 1. Kafka-Based Transaction Ingestion
Consumes transaction events from a Kafka topic.
Uses Spring Kafka with JSON serialization/deserialization.
Event-driven processing architecture.

✅ 2. Transaction Validation Engine
Transactions are considered valid only if:
Sender exists
Recipient exists
Sender has sufficient balance
Invalid transactions are discarded safely.

✅ 3. SQL Persistence (H2 + JPA)
Integrated H2 in-memory database
UserRecord and TransactionRecord entities
Proper Many-to-One relationships
ACID-compliant SQL guarantees

✅ 4. Incentive API Integration
Uses Spring RestTemplate
Sends validated transactions to external Incentive API
Receives incentive amount
Adds incentive only to recipient balance (not deducted from sender)

✅ 5. REST API for Balance Queries
Exposes:
GET /balance?userId={id}

Returns:
{
  "balance": 1234
}

Runs on port 33400
Returns 0 if user does not exist

## 🧱 System Architecture
           +-------------------+
           |   Kafka Topic     |
           |  trader-updates   |
           +---------+---------+
                     |
                     v
        +--------------------------+
        |   KafkaListenerService   |
        |  (Transaction Consumer)  |
        +-----------+--------------+
                    |
                    v
        +--------------------------+
        |   Validation Logic       |
        |  - User exists           |
        |  - Balance check         |
        +-----------+--------------+
                    |
                    v
        +--------------------------+
        |  Incentive API           |
        |  (External REST Service) |
        +-----------+--------------+
                    |
                    v
        +--------------------------+
        |  H2 Database (JPA)      |
        |  - Users                |
        |  - Transactions         |
        +-----------+--------------+
                    |
                    v
        +--------------------------+
        |  BalanceController       |
        |  GET /balance endpoint   |
        +--------------------------+

## 🛠 Tech Stack

Java 17
Spring Boot 3
Spring Kafka
Spring Data JPA
H2 In-Memory Database
RestTemplate
Maven
Embedded Kafka (Testing)

## 📦 Project Structure
src/
 ├── main/
 │   ├── java/com/jpmc/midascore/
 │   │   ├── KafkaListenerService
 │   │   ├── KafkaProducer
 │   │   ├── BalanceController
 │   │   ├── entity/
 │   │   ├── repository/
 │   │   └── foundation/
 │   └── resources/
 │       └── application.yml
 │
 └── test/
     ├── TaskTwoTests
     ├── TaskThreeTests
     ├── TaskFourTests
     └── TaskFiveTests

## ⚙️ Configuration
application.yml
server:
  port: 33400

spring:
  datasource:
    url: jdbc:h2:mem:testdb
  jpa:
    hibernate:
      ddl-auto: create-drop
  kafka:
    consumer:
      group-id: midas-core-group
      auto-offset-reset: earliest

## ▶️ How To Run
1️⃣ Start Incentive API

From project root:
java -jar services/transaction-incentive-api.jar

Make sure it runs on port 8080.

2️⃣ Run Midas Core
mvn clean spring-boot:run
Or run tests:
mvn test

3️⃣ Test Balance Endpoint
GET http://localhost:33400/balance?userId=1

## 🧪 Testing

This project includes:
Embedded Kafka integration tests
Database validation tests
Incentive API integration tests
REST API verification tests

Run specific test:
mvn test -Dtest=TaskFiveTests

## 🧠 Key Learnings

Event-driven backend design
Kafka consumer/producer configuration
Financial validation logic
SQL vs NoSQL architectural decisions
JPA entity relationships (Many-to-One)
External API integration via REST
Exposing REST endpoints in Spring Boot
Microservice-style system boundaries

## 💼 Resume-Ready Description
Built an event-driven financial transaction processor using Spring Boot, Kafka, JPA, and H2. Implemented validation rules, integrated with an external incentive API, persisted transactions in a relational database, and exposed REST endpoints for balance queries.

## 📌 Future Improvements
Replace H2 with PostgreSQL for production
Add transactional consistency with @Transactional
Add Swagger/OpenAPI documentation
Add Docker support
Add authentication layer
Introduce layered architecture (Service layer separation)

## 👨‍💻 Author
Utkarsh Raj

Backend Developer | Kafka | Spring Boot | Event-Driven Systems
