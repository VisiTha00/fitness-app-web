# Fitness App Microservices

A comprehensive fitness tracking application built with microservices architecture using Spring Boot and React.

## 🏗️ Architecture Overview

This application consists of multiple microservices that communicate through Eureka service discovery, API Gateway, and RabbitMQ message broker.

### Services

| Service              | Port | Description                             | Database   |
| -------------------- | ---- | --------------------------------------- | ---------- |
| **Eureka Server**    | 8761 | Service Discovery & Registry            | -          |
| **Config Server**    | 8888 | Centralized Configuration Management    | -          |
| **API Gateway**      | 8080 | Single Entry Point with OAuth2 Security | -          |
| **User Service**     | 8081 | User Management & Authentication        | PostgreSQL |
| **Activity Service** | 8082 | Fitness Activity Tracking               | MongoDB    |
| **AI Service**       | 8083 | AI-Powered Recommendations (Gemini)     | MongoDB    |
| **Frontend**         | 5173 | React Application (Vite)                | -          |

## 🚀 Technology Stack

### Backend

- **Framework:** Spring Boot 3.4.3
- **Java Version:** 23
- **Spring Cloud:** 2024.0.0
- **Service Discovery:** Netflix Eureka
- **API Gateway:** Spring Cloud Gateway
- **Message Broker:** RabbitMQ
- **Security:** OAuth2 + Keycloak (port 8181)

### Databases

- **PostgreSQL:** User Service
- **MongoDB:** Activity Service & AI Service

### Frontend

- **Framework:** React 19
- **Build Tool:** Vite 6.2.0
- **State Management:** Redux Toolkit 2.6.0
- **UI Library:** Material-UI (MUI) 6.4.6
- **Authentication:** OAuth2 PKCE
- **HTTP Client:** Axios 1.8.1

### AI Integration

- **Gemini API:** For AI-powered fitness recommendations

## 📋 Prerequisites

Before running the application, ensure you have the following installed:

- Java 23 or higher
- Maven 3.6+
- Node.js 18+ and npm
- PostgreSQL 14+
- MongoDB 6+
- RabbitMQ 3.11+
- Keycloak 21+ (configured on port 8181)

## 🔧 Setup Instructions

### 1. Database Setup

#### PostgreSQL (User Service)

```sql
CREATE DATABASE fitness_user_db;
-- Username: postgres
-- Password: admin@123
```

#### MongoDB

```bash
# Activity Service Database
mongo
use fitnessactivity

# AI Service Database
use fitnessrecommendation
```

### 2. RabbitMQ Setup

```bash
# Default credentials
# Host: localhost
# Port: 5672
# Username: guest
# Password: guest
```

### 3. Keycloak Setup

- Install and start Keycloak on port 8181
- Create a realm named: `fitness-oauth2`
- Configure OAuth2 clients for the application

### 4. Environment Variables

Set the following environment variables for the AI Service:

```bash
export GEMINI_API_URL=<your-gemini-api-url>
export GEMINI_API_KEY=<your-gemini-api-key>
```

## 🏃 Running the Application

### Start Order (Important!)

1. **Eureka Server** (Service Registry)

```bash
cd eureka
./mvnw spring-boot:run
```

Access at: http://localhost:8761

2. **Config Server** (Configuration Management)

```bash
cd configserver
./mvnw spring-boot:run
```

Access at: http://localhost:8888

3. **API Gateway**

```bash
cd gateway
./mvnw spring-boot:run
```

Access at: http://localhost:8080

4. **Microservices** (Can be started in parallel)

**User Service:**

```bash
cd userservice
./mvnw spring-boot:run
```

**Activity Service:**

```bash
cd activityservice
./mvnw spring-boot:run
```

**AI Service:**

```bash
cd aiservice
./mvnw spring-boot:run
```

5. **Frontend**

```bash
cd fitness-app-frontend
npm install
npm run dev
```

Access at: http://localhost:5173

## 📡 API Gateway Routes

All client requests should go through the API Gateway (port 8080):

| Route                     | Target Service   | Description                |
| ------------------------- | ---------------- | -------------------------- |
| `/api/users/**`           | User Service     | User management operations |
| `/api/activities/**`      | Activity Service | Fitness activity tracking  |
| `/api/recommendations/**` | AI Service       | AI-powered recommendations |

## 🔐 Security

- OAuth2 Resource Server with JWT validation
- Keycloak integration for authentication
- JWK Set URI: `http://localhost:8181/realms/fitness-oauth2/protocol/openid-connect/certs`

## 📨 Message Queue

### RabbitMQ Configuration

- **Exchange:** `fitness.exchange`
- **Queue:** `activity.queue`
- **Routing Key:** `activity.tracking`

Used for asynchronous communication between Activity Service and AI Service.

## 📁 Project Structure

```
fitness-app-microservices/
├── activityservice/       # Activity tracking service
├── aiservice/            # AI recommendations service
├── userservice/          # User management service
├── gateway/              # API Gateway
├── configserver/         # Configuration server
├── eureka/               # Service registry
├── fitness-app-frontend/ # React frontend
└── README.md
```
