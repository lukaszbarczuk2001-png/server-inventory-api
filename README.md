# Server Inventory API

REST API for keeping an inventory of servers across environments (DEV, TEST, STAGING, PROD). Each server has a hostname, IP address, operating system, CPU/RAM and a status.

Java 17, Spring Boot 4.1, Maven.

## Stack

- Spring Web
- Spring Data JPA / Hibernate
- H2 (in-memory, seeded with sample data on startup)
- Bean Validation
- springdoc-openapi (Swagger UI)
- Lombok
- JUnit 5, Mockito, MockMvc

## Running

Requires JDK 17+.

```
./mvnw spring-boot:run
```

On Windows use `mvnw.cmd spring-boot:run`, or run `ServerInventoryApiApplication` from the IDE.

- API: http://localhost:8080/api/servers
- Swagger UI: http://localhost:8080/swagger-ui.html
- H2 console: http://localhost:8080/h2-console (JDBC URL `jdbc:h2:mem:inventorydb`, user `sa`, no password)

## Tests

```
./mvnw test
```

## Endpoints

```
GET    /api/servers                 list servers, optional ?environment= and ?status= filters
GET    /api/servers/{id}            get one server
POST   /api/servers                 create a server
PUT    /api/servers/{id}            update a server
PATCH  /api/servers/{id}/status     change status only
DELETE /api/servers/{id}            delete a server
```

Example request body for POST / PUT:

```json
{
  "hostname": "cache-prod-01",
  "ipAddress": "10.0.6.10",
  "environment": "PROD",
  "operatingSystem": "Ubuntu 24.04 LTS",
  "cpuCores": 4,
  "ramGb": 32
}
```

`status` is optional (`RUNNING`, `STOPPED`, `MAINTENANCE`, `DECOMMISSIONED`). New servers default to `RUNNING`.

Errors are returned as Problem Details JSON:

- 400 – validation error or invalid parameter
- 404 – server not found
- 409 – hostname already exists

## Project structure

```
src/main/java/com/barczuk/serverinventory
├── controller
├── service
├── repository
├── model
├── dto
└── exception
```
