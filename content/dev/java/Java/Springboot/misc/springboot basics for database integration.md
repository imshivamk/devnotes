1. Model 
	1. defines "what is a note"
	2. its fields, rules, mapping to DB
	3. One source of truth to all data
	4. Like classes and objects as a rough analogy
2. Repository
	1. handles basic CRUD operations
	2. don't write SQL for every basic things
	3. we use ready methods
3. Service 
	1. Encapsulates business logic e.g. validation
	2. Reuse code and keep controllers slim
4. Controller
	1. Exposes REST endpoints (URLs)
	2. Converts http requests to method calls
	3. Organize your API, talks to client
5. Configs
	1. wires up backend to postgresql
	2. sets up framework config
	3. makes the environment management automatic and clear
## 1. Why Each Layer Exists (Architectural Breakdown)

|Layer|Responsibility|Why Do It This Way?|
|---|---|---|
|Model|Defines “what is a Note”—its fields, rules, mapping to DB.|One source of truth for your data.|
|Repository|Handles basic data operations (CRUD: Create, Read, Update, Delete).|Don’t write SQL for every basic thing; use ready methods.|
|Service|(Optional but common) Encapsulates business logic, e.g., validation.|Reuse code, keep controllers slim.|
|Controller|Exposes REST endpoints (URLs), converts HTTP to method calls.|Organizes your API, talks to the outside world (browser/frontend).|
|Configs|Wires up backend to PostgreSQL, sets up framework config.|Make environment/settings management automatic and clear.|
## 2. Key Java Spring Boot Annotations & Concepts

###  `@Entity`

Java class that maps to a database table.

```java
@Entity
public class Note{
	...
}
```

- Tells JPA/Hibernate : "This is a table, generate DB structure from it's fields"

###  `@Id` and `@GeneratedValue`

Marks field as the primary key and makes it auto-increment.

```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

### `@Column`

 Customizes column names or settings (not always needed; by default, fields match columns).

### `@Repository`

Marks interface/class for dependency injection, exception translation (not strictly needed for pure interfaces extending JpaRepository, but good practice).

### `@RestController`

Marks class as a REST controller: all its methods are HTTP endpoints and return JSON.

### `@RequestMapping`, `@GetMapping`, `@PostMapping`, etc.

Maps HTTP methods and URLs to Java methods.

### `@Autowired`

Tells Spring to inject (pass in) a dependency automatically.

## 3. The Repository Pattern: What Is `NoteRepository`?

```java
@Repository
public interface NoteRepository extends JpaRepository<Note, Long> { }
```

- JpaRepository is Spring data interface : it gives you built-in CRUD operations with zero code - `save`, `find`, `delete` etc.
- You don't need to write sql for common actions like - `findById()`, `all()`, `save()`
- Type safe : Specifies what entity (Note) and kind of ID you use.
- you can call `noteRepository.findAll() ` to get all notes without manually writing sql
## 4. Service Layer (Why Have One?)

- Keeps business logic (validation, combining data, more than just fetch-or-save) out of your controller.
- Example responsibilities:
    - Check if note title is unique before saving.
    - Limit note content length before update.
    - Any reusable business rule or transformation.
## 5. Controller Layer: What It Does

```java
@RestController
@RequestMapping("api/notes")

public class NotesController{
	...
}
```

- receives HTTP requests front client
- Delegates work to the Service (or Directly to repository)
- Turns Java objects to JSON and vice versa (using Json, auto-wired, gson)
- Decouples HTTP/web layer from your application code.
- Makes APIs self-documenting and organized.

### CRUD Methods (examples)

- `@GetMapping("")` → get all notes
- `@GetMapping("/{id}")` → get a note by ID
- `@PostMapping("")` → create new note
- `@PutMapping("/{id}")` → update note by ID
- `@DeleteMapping("/{id}")` → delete note by ID

## 6. application.properties File

`application.properties`

```
spring.datasource.url=jdbc:postgresql://localhost:5432/notesdb
spring.datasource.username=youruser
spring.datasource.password=yourpassword
```

```
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

- Tells Spring how to connect to your PostgreSQL DB.
- `ddl-auto=update`: Updates the database schema to match your model (handy for development).

## 7. Dependency Injection

- Spring automatically provides (“injects”) your repository/service beans where needed (`@Autowired`), so you don’t have to construct these every time.
- - This keeps code modular/testable and leverages “Inversion of Control”—the container handles wiring and lifecycle.
