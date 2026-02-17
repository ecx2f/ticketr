# Ticketr

Simple event ticket system. Web-based sales and management with role-based access, real-time availability, and concurrency-safe purchase handling.

### Description

Ticketr provides:

- Two roles: normal user and administrator
- Real-time ticket availability
- Concurrency control to prevent overselling

### Stack

| Technology       | Version | Purpose                    |
|------------------|---------|----------------------------|
| Java             | 21      | Runtime                    |
| Spring Boot      | 3.x     | Backend                    |
| Thymeleaf        | 3.1     | Server-side templates      |
| Bootstrap        | 5.3     | UI                         |
| MySQL            | 8.0+    | Database                   |
| Spring Security  | 6.x     | Auth and authorization     |

### Architecture

```
Browser  -->  Spring Boot (MVC)  -->  MySQL
```

### Package layout

```
src/main/java/com/example/sistemaboletos/
  config/       Security and app configuration
  controller/   HTTP handlers and navigation
  model/        Entities (Usuario, Evento, Compra, Rol, EntidadBase)
  model/servicio/  Service interfaces and implementations
  repository/   JPA data access
  SistemaBoletosApplication.java
```

### Features

**Auth**

- Login and registration
- Default admin account: `admin@admin.com` / `admin123`

**Events (admin only)**

- CRUD at `/admin/eventos`
- List: `GET /admin/eventos`
- Save: `POST /admin/eventos/guardar`
- Delete: `GET /admin/eventos/eliminar/{id}`

**Purchases**

- Concurrency-safe ticket purchase via `@Transactional` and `synchronized` in `EventoServiceImpl.comprarBoletos()`

### Run

**Prerequisites:** JDK 21+, MySQL 8.0+, Maven

1. Create database:

   ```sql
   CREATE DATABASE sistema_boletos;
   ```

2. Set connection in `src/main/resources/application.properties`:

   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/sistema_boletos
   spring.datasource.username=your_user
   spring.datasource.password=your_password
   ```

3. Start the app:

   ```bash
   mvn spring-boot:run
   ```

### Requirements checklist

| Area           | Requirement           | How it is met |
|----------------|-----------------------|---------------|
| Architecture   | Client–server         | Spring Boot backend, Thymeleaf/Bootstrap frontend |
| Concurrency    | Thread safety         | `@Transactional` + `synchronized` in `EventoServiceImpl.comprarBoletos()` ([code](src/main/java/com/example/sistemaboletos/model/servicio/EventoServiceImpl.java)) |
| Security       | Authentication        | Spring Security; `/admin/**` restricted by role |
| Persistence    | Full CRUD             | JPA repositories for Users, Events, Purchases, Tickets |
| Validation     | Error handling        | Purchase validation and Spring Security error views (e.g. `login?error`) |
| Data structures| Generic collections   | `List<T>`, `Optional<T>`, Security context maps |
| OOP            | Abstract class        | `EntidadBase` for shared `id` |
| OOP            | Enum                  | `Rol` (USER, ADMIN) |
| OOP            | Interface             | `IEventoService` implemented by `EventoServiceImpl` |

Spanish version: [README_ES.md](README_ES.md)
