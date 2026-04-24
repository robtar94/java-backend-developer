# Appointment Booking App Project

This project was initialy created during an internal training organized by my employer.

## Table of Contents

- [Live Demo (VPS)](#live-demo-vps)
- [Login Credentials](#login-credentials)
- [Technology Stack](#technology-stack)
- [Java Version](#java-version)
- [Used Libraries](#used-libraries)
- [Available Endpoints](#available-endpoints)

## Live Demo (VPS)

The application is deployed and available at: **http://srv11.mikr.us:20109/**

Current deployed version: **0.0.2.0-ALPHA** (see `pom.xml`).

## Login Credentials

The following demo accounts are available (both on the VPS demo and locally):

| Role         | Username      | Password      |
|--------------|---------------|---------------|
| Admin        | `admin`       | `admin`       |
| Specialist   | `specialist`  | `specialist`  |
| Receptionist | `receptionist`| `receptionist`|
| Client       | `client`      | `client`      |

## Technology Stack

### Backend

- **Java 21**
- **Spring Boot 3.5** (Web, Data JPA, Security, Validation, Actuator)
- **Spring Security** (Basic Auth, in-memory users)
- **Hibernate / JPA** with **QueryDSL** for type-safe queries
- **MapStruct** for DTO/entity mapping
- **Flyway** for database migrations
- **H2** in-memory database
- **OpenAPI / Swagger** (springdoc + openapi-generator-maven-plugin)
- **Lombok**
- **Maven** as the build tool
- **JUnit 5** and **Spring Security Test** for testing
- **Docker** (see `Dockerfile`) for containerized deployment on VPS

### Frontend

- **React** + **Vite** (bundled static assets served from `src/main/resources/static`)
- **Axios** for HTTP calls to the REST API
- Static HTML pages (`login.html`, `home.html`, `treatments.html`, `appointments.html`) integrated with the Spring Boot backend

## Java Version

This project uses Java version: **21**

## Used Libraries

List of libraries used in this project along with their versions:

- spring-boot-starter-web: 3.5.13
- spring-boot-starter-data-jpa: 3.5.13
- spring-boot-starter-security: 3.5.13
- spring-boot-starter-validation: 3.5.13
- spring-boot-starter-actuator: 3.5.13
- springdoc-openapi-starter-webmvc-ui: 2.8.5
- openapi-generator-maven-plugin: 7.11.0
- swagger-annotations: 2.2.28
- jackson-databind-nullable: 0.2.6
- mapstruct: 1.5.5.Final
- querydsl-jpa: 5.1.0
- flyway-core (managed by Spring Boot)
- h2 (managed by Spring Boot)
- lombok: 1.18.30
- spring-boot-starter-test: 3.5.13
- spring-security-test: 3.5.13
- junit-jupiter-api: 5.10.2

## Available Endpoints

### List treatments

- **URL:** `/api/v1/treatments`
- **Method:** `GET`
- **Auth required:** Yes (Basic Auth)

#### Success Response

- **Code:** `200 OK`
- **Content:**

```json
[
    {
        "id": -1,
        "name": "Konsultacja dentystyczna",
        "duration": 30,
        "specialistId": -1
    }
]
```

### Get treatment details

- **URL:** `/api/v1/treatments/{id}`
- **Method:** `GET`
- **Auth required:** Yes (Basic Auth)

#### Success Response

- **Code:** `200 OK`
- **Content:**

```json
{
    "id": -1,
    "name": "Konsultacja dentystyczna",
    "description": "Konsultacja dentystyczna z diagnostyką i planem leczenia",
    "duration": 30,
    "specialist": {
        "id": -1,
        "name": "DENTIST"
    }
}
```

#### Error Response

- **Code:** `404 NOT FOUND`
- **Content:**

```json
{
    "status": "404",
    "message": "Treatment has not been found"
}
```

### Create treatment

- **URL:** `/api/v1/treatments`
- **Method:** `POST`
- **Auth required:** Yes (Basic Auth, role `SPECIALIST`)
- **Body:**

```json
{
    "name": "Konsultacja dentystyczna",
    "description": "Konsultacja dentystyczna z diagnostyką i planem leczenia",
    "duration": 30,
    "specialistId": -1
}
```

### List appointments

- **URL:** `/api/v1/appointments`
- **Method:** `GET`
- **Auth required:** Yes (Basic Auth)

#### Success Response

- **Code:** `200 OK`
- **Content:**

```json
[
    {
        "id": -1,
        "clientId": -1,
        "treatmentId": -1,
        "dateTime": "2024-03-01T09:00:00Z",
        "status": "SCHEDULED"
    }
]
```

### Book a new appointment

- **URL:** `/api/v1/appointments`
- **Method:** `POST`
- **Auth required:** Yes (Basic Auth)
- **Body:**

```json
{
    "clientId": -1,
    "treatmentId": -1,
    "dateTime": "2024-06-01T09:00:00Z"
}
```

#### Error Response

- **Code:** `400 BAD REQUEST`
- **Content:**

```json
{
    "status": "400",
    "message": "Specialist is not available at this time"
}
```

### Update appointment status (cancel / complete)

- **URL:** `/api/v1/appointments/{id}`
- **Method:** `PATCH`
- **Auth required:** Yes (Basic Auth)
- **Body:**

```json
{
    "status": "CANCELLED"
}
```

### Current authenticated user info

- **URL:** `/api/user/info`
- **Method:** `GET`
- **Auth required:** Yes (Basic Auth)

#### Success Response

- **Code:** `200 OK`
- **Content:**

```json
{
    "authenticated": true,
    "username": "client",
    "roles": ["CLIENT"],
    "id": -1,
    "clientId": -1
}
```

All responses are in `application/json` format.
