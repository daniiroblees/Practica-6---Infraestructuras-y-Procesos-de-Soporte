# SpringUMA — Medical Records System

> ⚠️ Replace `daniiroblees` and `https://github.com/daniiroblees/Practica-6---Infraestructuras-y-Procesos-de-Soporte` with your actual GitHub account details.

A Spring Boot REST API that models a small medical records system. Built as a practice project for the **Software Maintenance and Testing** course at the University of Málaga.

---

## Domain Model

| Entity    | Description                                                             |
|-----------|-------------------------------------------------------------------------|
| `Medico`  | Medical doctor with `id`, `nombre`, `dni`, `especialidad`               |
| `Paciente`| Patient with `id`, `nombre`, `dni`, `edad`, `cita`, assigned doctor     |
| `Imagen`  | Medical image (binary, compressed) linked to a patient                  |
| `Informe` | AI-generated prediction report linked to an image                       |

Relationships: `Medico` ←1:N→ `Paciente` ←1:N→ `Imagen` ←1:N→ `Informe`

---

## Project Structure

```
src/
├── main/java/com/uma/example/springuma/
│   ├── controller/
│   │   ├── MedicoController.java
│   │   ├── PacienteController.java
│   │   ├── ImagenController.java
│   │   └── InformeController.java
│   ├── model/               # JPA entities, services and repositories
│   └── utils/
│       └── ImageUtils.java  # Compress / decompress helpers
└── test/java/com/uma/example/springuma/
    └── integration/
        ├── base/AbstractIntegration.java              # Shared Spring Boot test config
        ├── MedicoControllerMockMvcIT.java             # Doctor CRUD via MockMvc
        ├── PacienteControllerMockMvcIT.java           # Patient CRUD via MockMvc
        ├── ImagenControllerWebTestClientIT.java       # Image upload/download via WebTestClient
        └── InformeControllerWebTestClientIT.java      # Report lifecycle via WebTestClient
```

---

## Requirements

| Tool  | Min version |
|-------|-------------|
| Java  | 21          |
| Maven | 3.8+        |

No external database required — H2 in-memory is used for tests.

---

## Running the application

```bash
./mvnw spring-boot:run
```

API available at `http://localhost:8080`.  
Swagger UI: `http://localhost:8080/swagger-ui.html`

### Main endpoints

| Method | Path                      | Description                          |
|--------|---------------------------|--------------------------------------|
| GET    | `/medico/{id}`            | Get doctor by ID                     |
| GET    | `/medico/dni/{dni}`       | Get doctor by DNI                    |
| POST   | `/medico`                 | Create doctor                        |
| PUT    | `/medico`                 | Update doctor                        |
| DELETE | `/medico/{id}`            | Delete doctor                        |
| GET    | `/paciente/{id}`          | Get patient by ID                    |
| GET    | `/paciente/medico/{id}`   | List patients by doctor              |
| POST   | `/paciente`               | Create patient                       |
| PUT    | `/paciente`               | Update patient                       |
| DELETE | `/paciente/{id}`          | Delete patient                       |
| POST   | `/imagen`                 | Upload image (multipart)             |
| GET    | `/imagen/{id}`            | Download image bytes                 |
| GET    | `/imagen/info/{id}`       | Get image metadata                   |
| GET    | `/imagen/predict/{id}`    | Get AI prediction for image          |
| GET    | `/imagen/paciente/{id}`   | List images for a patient            |
| DELETE | `/imagen/{id}`            | Delete image                         |
| GET    | `/informe/{id}`           | Get report by ID                     |
| GET    | `/informe/imagen/{id}`    | List reports for an image            |
| POST   | `/informe`                | Create report                        |
| DELETE | `/informe/{id}`           | Delete report                        |

---

## Running tests

```bash
# Unit tests only
./mvnw test

# All tests (unit + integration)
./mvnw verify
```

---

## CI/CD — GitHub Actions

The workflow in `.github/workflows/ci.yml` triggers on every **push** and **pull request** and:

1. Compiles the project with JDK 21.
2. Runs unit tests (`maven-surefire-plugin`).
3. Runs integration tests (`maven-failsafe-plugin`, classes ending in `IT`).
4. Publishes a test report to the GitHub **Checks** tab via `dorny/test-reporter`.
5. Uploads XML reports as a downloadable artifact (retained 7 days).

---

## Technology Stack

- Java 21 · Spring Boot 3.2
- Spring Data JPA + H2
- JUnit 5 · MockMvc · WebTestClient
- SpringDoc OpenAPI (Swagger UI)
- Maven · GitHub Actions
