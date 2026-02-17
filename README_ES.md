# Ticketr

Sistema simple de venta y gestión de entradas para eventos. Aplicación web con acceso por roles, disponibilidad en tiempo real y compra segura ante concurrencia.

### Descripción

Ticketr incluye:

- Dos roles: usuario normal y administrador
- Disponibilidad de entradas en tiempo real
- Control de concurrencia para evitar sobreventa

### Tecnologías

| Tecnología      | Versión | Uso                         |
|-----------------|---------|-----------------------------|
| Java            | 21      | Lenguaje y runtime          |
| Spring Boot     | 3.x     | Backend                     |
| Thymeleaf       | 3.1     | Plantillas HTML             |
| Bootstrap       | 5.3     | Interfaz responsive         |
| MySQL           | 8.0+    | Base de datos               |
| Spring Security | 6.x     | Autenticación y autorización|

### Arquitectura

```
Navegador  -->  Spring Boot (MVC)  -->  MySQL
```

### Estructura de paquetes

```
src/main/java/com/example/sistemaboletos/
  config/       Configuración y seguridad
  controller/   Controladores y rutas
  model/        Entidades (Usuario, Evento, Compra, Rol, EntidadBase)
  model/servicio/  Interfaces e implementaciones de servicios
  repository/   Acceso a datos JPA
  SistemaBoletosApplication.java
```

### Funcionalidades

**Autenticación**

- Inicio de sesión y registro
- Cuenta admin por defecto: `admin@admin.com` / `admin123`

**Eventos (solo administrador)**

- CRUD en `/admin/eventos`
- Listar: `GET /admin/eventos`
- Guardar: `POST /admin/eventos/guardar`
- Eliminar: `GET /admin/eventos/eliminar/{id}`

**Compras**

- Compra de boletos con manejo de concurrencia mediante `@Transactional` y `synchronized` en `EventoServiceImpl.comprarBoletos()`.

### Ejecución

**Requisitos:** JDK 21+, MySQL 8.0+, Maven

1. Crear la base de datos:

   ```sql
   CREATE DATABASE sistema_boletos;
   ```

2. Configurar la conexión en `src/main/resources/application.properties`:

   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/sistema_boletos
   spring.datasource.username=tu_usuario
   spring.datasource.password=tu_contraseña
   ```

3. Arrancar la aplicación:

   ```bash
   mvn spring-boot:run
   ```

### Cumplimiento de requisitos

| Área            | Requisito              | Implementación |
|-----------------|------------------------|----------------|
| Arquitectura    | Cliente–servidor       | Spring Boot en backend, Thymeleaf/Bootstrap en frontend |
| Concurrencia    | Manejo de hilos        | `@Transactional` + `synchronized` en `EventoServiceImpl.comprarBoletos()` ([código](src/main/java/com/example/sistemaboletos/model/servicio/EventoServiceImpl.java)) |
| Seguridad       | Autenticación          | Spring Security; rutas `/admin/**` restringidas por rol |
| Persistencia    | CRUD completo          | Repositorios JPA para Usuarios, Eventos, Compras, Boletos |
| Validaciones    | Manejo de errores      | Validación en compras y vistas de error de Spring Security (p. ej. `login?error`) |
| Estructuras     | Colecciones genéricas  | `List<T>`, `Optional<T>`, mapas del contexto de seguridad |
| POO             | Clase abstracta        | `EntidadBase` con campo `id` común |
| POO             | Enum                   | `Rol` (USER, ADMIN) |
| POO             | Interfaz               | `IEventoService` implementada por `EventoServiceImpl` |

English version: [README.md](README.md)
