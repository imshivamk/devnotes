
# Part 1 — What did a Java backend look like before Spring?

Let's imagine we're building a simple backend:

```text
POST /users
GET  /users/42
DELETE /users/42
```

with:

```text
Java
   ↓
HTTP server
   ↓
REST API
   ↓
Business logic
   ↓
PostgreSQL
```

Today you'd probably use Spring Boot.

Historically, a common Java web stack was roughly:

```text
Browser / React
       ↓
HTTP
       ↓
Servlet Container
(Tomcat)
       ↓
Servlet
       ↓
Java code
       ↓
JDBC
       ↓
Database
```

The key technology here is the **Servlet API**.

---

# 1. What is a Servlet?

A servlet is essentially a Java class that can receive and respond to HTTP requests.

For example:

```java
public class UserServlet extends HttpServlet {

    @Override
    protected void doGet(
            HttpServletRequest request,
            HttpServletResponse response)
            throws IOException {

        response.setContentType("application/json");

        response.getWriter().write(
            "{\"id\":1,\"name\":\"Shivam\"}"
        );
    }
}
```

A client sends:

```http
GET /users/1
```

Tomcat receives it.

Tomcat finds the appropriate servlet.

Then:

```text
HTTP Request
     ↓
Tomcat
     ↓
UserServlet
     ↓
doGet()
     ↓
HTTP Response
```

The servlet API gives you objects such as:

```java
HttpServletRequest
HttpServletResponse
```

So you can access:

```java
request.getMethod();
request.getParameter("id");
request.getHeader("Authorization");
request.getCookies();
```

and write:

```java
response.setStatus(200);
response.setContentType("application/json");
response.getWriter().write(...);
```

---

# 2. How did routing work?

You need to tell the servlet container:

> When `/users/*` is requested, send the request to `UserServlet`.

Historically this could be configured in `web.xml`.

Something like:

```xml
<servlet>
    <servlet-name>UserServlet</servlet-name>
    <servlet-class>com.example.UserServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>UserServlet</servlet-name>
    <url-pattern>/users/*</url-pattern>
</servlet-mapping>
```

So:

```text
GET /users/42
       ↓
Tomcat
       ↓
URL mapping
       ↓
UserServlet
```

Spring MVC eventually provides a much nicer abstraction over this.

---

# 3. But REST means JSON — how did we create JSON?

This is another important point.

The Servlet API itself doesn't magically turn Java objects into JSON.

You might manually write:

```java
response.getWriter().write(
    "{\"id\":1,\"name\":\"Shivam\"}"
);
```

Obviously this becomes horrible for real applications.

So libraries such as **Jackson** became commonly used.

You could have:

```java
User user = new User(1L, "Shivam");
```

and serialize it:

```java
ObjectMapper mapper = new ObjectMapper();

String json = mapper.writeValueAsString(user);

response.getWriter().write(json);
```

Result:

```json
{
  "id": 1,
  "name": "Shivam"
}
```

Spring Boot later integrates Jackson for you.

That's why you can eventually write:

```java
@GetMapping("/users/{id}")
public User getUser(...) {
    return user;
}
```

and Spring handles the conversion to JSON.

---

# 4. Now the database problem

Suppose `/users/42` should retrieve this:

```text
User
id = 42
name = Shivam
email = shivam@example.com
```

Before Spring Data JPA, a common approach was **JDBC**.

JDBC = **Java Database Connectivity**.

It's essentially Java's standard API for talking to relational databases.

You might write:

```java
Connection connection = DriverManager.getConnection(
    url,
    username,
    password
);
```

Then:

```java
PreparedStatement statement =
    connection.prepareStatement(
        "SELECT id, name, email FROM users WHERE id = ?"
    );

statement.setLong(1, 42L);
```

Execute:

```java
ResultSet resultSet =
    statement.executeQuery();
```

Then manually read:

```java
if (resultSet.next()) {

    Long id = resultSet.getLong("id");
    String name = resultSet.getString("name");
    String email = resultSet.getString("email");

    User user = new User(
        id,
        name,
        email
    );
}
```

Finally:

```java
resultSet.close();
statement.close();
connection.close();
```

---

# 5. Imagine doing that for every endpoint

Your application starts becoming:

```text
Servlet
  ↓
JDBC
  ↓
SQL
  ↓
ResultSet
  ↓
Object mapping
```

For every operation:

```java
Connection
PreparedStatement
ResultSet
try/catch
close resources
map columns → object
```

And you need to handle:

- database connections
    
- connection pooling
    
- transactions
    
- exceptions
    
- SQL
    
- object mapping
    
- JSON
    
- routing
    
- authentication
    
- authorization
    
- logging
    
- validation
    

The problem isn't that any individual operation is impossible.

The problem is **you repeatedly write infrastructure code instead of application code**.

---

# 6. What did a traditional architecture look like?

Developers still separated concerns.

For example:

```text
UserServlet
     ↓
UserService
     ↓
UserDAO
     ↓
JDBC
     ↓
PostgreSQL
```

### Servlet

Handles HTTP:

```java
public class UserServlet extends HttpServlet {

    private UserService userService;

    protected void doGet(...) {
        ...
    }
}
```

### Service

Business logic:

```java
public class UserService {

    private UserDAO userDAO;

    public User getUser(Long id) {
        return userDAO.findById(id);
    }
}
```

### DAO

Database operations:

```java
public class UserDAO {

    public User findById(Long id) {

        // JDBC
        // SQL
        // ResultSet
        // Mapping

    }
}
```

This architecture is actually **pretty good conceptually**.

The problem was the amount of infrastructure required to wire everything together.

---

# 7. Who creates these objects?

This is where we arrive at the fundamental problem Spring solves.

You have:

```java
UserServlet servlet;
UserService service;
UserDAO dao;
```

And dependencies:

```text
UserServlet
     ↓
UserService
     ↓
UserDAO
```

Someone has to create them.

You might manually do:

```java
UserDAO dao = new UserDAO();

UserService service =
    new UserService(dao);

UserServlet servlet =
    new UserServlet(service);
```

Notice what's happening.

**Your application is responsible for constructing the entire object graph.**

For a small application, that's fine.

But imagine:

```text
UserController
      ↓
UserService
      ↓
UserRepository
      ↓
Database

OrderController
      ↓
OrderService
      ↓
OrderRepository
      ↓
PaymentService
      ↓
PaymentRepository

AuthController
      ↓
AuthService
      ↓
UserRepository
      ↓
PasswordEncoder
      ↓
TokenService
```

You eventually have hundreds of objects and dependencies.

Manually managing this becomes painful.

And this is where **Spring Core begins**.

---

# Part 2 — Spring Core

The central idea of Spring is:

> **Don't make every class responsible for creating and managing the objects it depends on. Let a container manage those objects and inject the dependencies.**

This sounds abstract.

Let's build the idea step by step.

---

# 8. First concept: Dependency

Suppose:

```java
class UserService {

    private UserRepository repository;

}
```

`UserService` needs `UserRepository`.

Therefore:

```text
UserService
    ↓ depends on
UserRepository
```

`UserRepository` is a **dependency** of `UserService`.

That's all "dependency" means.

---

# 9. The bad approach: creating your own dependency

You could do:

```java
class UserService {

    private UserRepository repository;

    UserService() {
        repository = new UserRepository();
    }
}
```

Looks harmless.

But now `UserService` is tightly coupled to the concrete implementation.

What if you want:

```text
PostgresUserRepository
```

instead of:

```text
UserRepository
```

Or a fake repository for testing?

You'd have to modify `UserService`.

---

# 10. Better approach: Dependency Injection

Instead:

```java
class UserService {

    private final UserRepository repository;

    UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

Now `UserService` doesn't create the repository.

Someone else gives it one.

That's **Dependency Injection**.

```text
             creates
               ↓
        UserRepository
               │
               │ injected into
               ↓
          UserService
```

Notice the inversion.

Before:

```text
UserService → creates → UserRepository
```

After:

```text
Some external system → creates → UserRepository
                              ↓
                         UserService
```

Spring is that external system.

---

# 11. IoC — Inversion of Control

This leads to one of the most important Spring terms:

**IoC = Inversion of Control.**

Normally your application controls object creation:

```java
UserRepository repo = new UserRepository();
UserService service = new UserService(repo);
```

With Spring:

```text
Spring Container
      │
      ├── creates UserRepository
      │
      └── creates UserService
              │
              └── injects UserRepository
```

Control over object creation has been **inverted**.

Instead of:

> "My application creates everything."

you say:

> "Spring manages my application's objects."

---

# 12. What is the Spring Container?

This is probably the most important mental model.

The **Spring Container** is an object-management system.

A major interface representing it is:

```java
ApplicationContext
```

Conceptually:

```text
ApplicationContext
       │
       ├── UserRepository
       ├── UserService
       ├── UserController
       ├── PaymentService
       ├── EmailService
       └── ...
```

Spring creates and manages these objects.

Objects managed by Spring are called:

# Beans

---

# 13. What is a Bean?

A **Spring Bean is simply an object managed by the Spring IoC container.**

For example:

```java
class UserService {
}
```

This is just a normal Java class.

If Spring creates and manages an instance of it:

```text
UserService object
      ↓
managed by Spring
      ↓
Spring Bean
```

That's it.

A bean isn't some special new kind of Java object.

It's still an ordinary Java object.

---

# 14. How do we tell Spring to manage a class?

One common way is:

```java
@Component
public class UserService {
}
```

`@Component` basically tells Spring:

> "Create and manage an instance of this class as a bean."

Spring scans your application:

```text
@Component
class UserService
```

and registers it.

Then you can inject it elsewhere.

---

# 15. Specialized annotations

Spring provides specialized versions of `@Component`.

### `@Service`

For business/service logic:

```java
@Service
public class UserService {
}
```

### `@Repository`

For persistence/database layer:

```java
@Repository
public class UserRepository {
}
```

### `@Controller`

For MVC controllers:

```java
@Controller
public class UserController {
}
```

### `@RestController`

For REST APIs:

```java
@RestController
public class UserController {
}
```

Conceptually:

```text
@Component
   │
   ├── @Service
   ├── @Repository
   ├── @Controller
   └── @RestController
```

They communicate intent and, in some cases, add framework-specific behavior.

---

# 16. Dependency Injection with Spring

Now:

```java
@Repository
public class UserRepository {
}
```

and:

```java
@Service
public class UserService {

    private final UserRepository repository;

    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

Spring sees:

```text
UserRepository → Bean
UserService     → Bean
```

And understands:

```text
UserService
    ↓
requires UserRepository
```

So it effectively does:

```java
UserRepository repository =
    new UserRepository();

UserService service =
    new UserService(repository);
```

**You don't write those `new` statements.**

Spring does the wiring.

---

# 17. Constructor Injection

The example above is called **constructor injection**.

```java
@Service
public class UserService {

    private final UserRepository repository;

    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

This is generally the preferred form.

Why?

Because `UserService` cannot exist without its required dependency.

The dependency is mandatory.

Also:

```java
private final UserRepository repository;
```

means the dependency cannot later be reassigned.

---

# 18. Field injection

You'll also see:

```java
@Autowired
private UserRepository repository;
```

Spring injects it directly into the field.

This works, but constructor injection is generally preferred because dependencies are explicit and the class is easier to test.

For example, constructor injection allows:

```java
UserRepository fakeRepository =
    new FakeUserRepository();

UserService service =
    new UserService(fakeRepository);
```

without needing Spring.

---

# 19. Now let's understand the whole chain

Suppose:

```java
@Repository
class UserRepository {
}
```

```java
@Service
class UserService {

    private final UserRepository repository;

    UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

```java
@RestController
class UserController {

    private final UserService service;

    UserController(UserService service) {
        this.service = service;
    }
}
```

Spring creates:

```text
Spring Container
       │
       ├── UserRepository
       │
       ├── UserService
       │       │
       │       └── UserRepository
       │
       └── UserController
               │
               └── UserService
```

You don't manually construct any of them.

---

# 20. Component Scanning

But how does Spring even find these classes?

Through **component scanning**.

Spring Boot starts your application.

It scans packages for classes annotated with things such as:

```java
@Component
@Service
@Repository
@Controller
```

For example:

```text
com.example.app
│
├── Application.java
│
├── controller
│    └── UserController.java
│
├── service
│    └── UserService.java
│
└── repository
     └── UserRepository.java
```

Spring scans the package and discovers the components.

Then it registers them as beans.

---

# 21. `@SpringBootApplication`

You'll commonly see:

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(
            Application.class,
            args
        );
    }
}
```

`@SpringBootApplication` is important because it combines several capabilities, notably:

```text
@Configuration
@EnableAutoConfiguration
@ComponentScan
```

So conceptually:

```text
Spring Boot starts
       ↓
Create ApplicationContext
       ↓
Component scanning
       ↓
Find beans
       ↓
Create beans
       ↓
Resolve dependencies
       ↓
Inject dependencies
       ↓
Application ready
```

---

# 22. `@Bean` — another way to create beans

You don't always want to put `@Component` on a class.

You can explicitly tell Spring:

```java
@Configuration
public class AppConfig {

    @Bean
    public UserService userService() {
        return new UserService();
    }
}
```

Here:

```java
@Bean
```

means:

> "The object returned by this method should be managed by Spring."

This is particularly useful for third-party classes that you can't modify.

For example:

```java
@Bean
public ObjectMapper objectMapper() {
    return new ObjectMapper();
}
```

Spring now manages that `ObjectMapper`.

---

# 23. `@Component` vs `@Bean`

This distinction is important.

### `@Component`

Put it on the class:

```java
@Component
class EmailService {
}
```

Spring discovers it through component scanning.

### `@Bean`

Put it on a method:

```java
@Configuration
class Config {

    @Bean
    EmailService emailService() {
        return new EmailService();
    }
}
```

You explicitly tell Spring how to create it.

Mental shortcut:

```text
@Component
"I want Spring to discover this class."

@Bean
"I want Spring to use this method to create this object."
```

---

# 24. What is `ApplicationContext` actually doing?

At startup, conceptually:

```text
                ApplicationContext
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
   UserRepository  UserService   UserController
                       │              │
                       ↓              ↓
                UserRepository   UserService
```

Spring maintains metadata describing:

```text
Bean name
Bean type
Dependencies
Scope
Lifecycle
Configuration
```

Then it creates objects and connects them.

This is the **IoC container**.

---

# 25. Singleton beans

By default, Spring beans have **singleton scope**.

For example:

```java
@Service
class UserService {
}
```

By default, Spring generally creates **one UserService instance per Spring ApplicationContext**.

Conceptually:

```text
Spring Container

UserService → Object #1
                  ↑
         Controller A
         Controller B
         Controller C
```

They can all use the same managed instance.

This doesn't mean Java's global Singleton pattern.

It means Spring's container maintains one bean instance for that scope.

---

# 26. Bean lifecycle

Spring doesn't merely do:

```java
new UserService();
```

and forget about it.

It manages the lifecycle.

Simplified:

```text
Application starts
       ↓
Spring discovers bean
       ↓
Creates object
       ↓
Injects dependencies
       ↓
Initialization
       ↓
Bean ready
       ↓
Application runs
       ↓
Application shuts down
       ↓
Bean destroyed
```

There are lifecycle hooks such as:

```java
@PostConstruct
```

for initialization.

For example:

```java
@Service
class CacheService {

    @PostConstruct
    void initialize() {
        System.out.println("Initializing cache...");
    }
}
```

---

# 27. Why does all this matter?

Because Spring gives you **separation of responsibilities**.

Without dependency injection:

```text
UserService
    │
    ├── creates repository
    ├── creates database connection
    ├── creates other services
    └── manages dependencies
```

With Spring:

```text
              Spring
             /  |  \
            /   |   \
           ↓    ↓    ↓
       Repository Service Controller
           │        │       │
           └────────┴───────┘
             dependencies
```

Your classes focus on their actual responsibilities.

---

# 28. The deeper idea: loose coupling

Consider:

```java
class UserService {

    private MySQLUserRepository repository;

    UserService() {
        repository = new MySQLUserRepository();
    }
}
```

Very tightly coupled.

Now:

```java
class UserService {

    private final UserRepository repository;

    UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

where:

```java
interface UserRepository {
    User findById(Long id);
}
```

Now:

```text
                 UserRepository
                  /          \
                 /            \
                ↓              ↓
     PostgresUserRepository   MockUserRepository
```

You can inject whichever implementation you need.

That's one of the biggest benefits of DI.

---

# 29. Dependency Injection makes testing easier

Suppose:

```java
@Service
class UserService {

    private final UserRepository repository;

    UserService(UserRepository repository) {
        this.repository = repository;
    }

    User getUser(Long id) {
        return repository.findById(id);
    }
}
```

During production:

```text
UserService
     ↓
PostgresUserRepository
```

During testing:

```text
UserService
     ↓
FakeUserRepository
```

You can test business logic without requiring a real PostgreSQL database.

This is a major reason DI exists.

---

# 30. Spring Core vs Spring Boot

Now you can see the distinction much more clearly.

### Spring Core gives you:

```text
IoC
Dependency Injection
Beans
ApplicationContext
Bean lifecycle
Scopes
Configuration
Component scanning
```

### Spring Boot gives you:

```text
Spring
  +
Automatic configuration
  +
Embedded server
  +
Starter dependencies
  +
Production-friendly defaults
  +
Easy application setup
```

So:

```text
                 Spring Boot
                     │
                     ↓
                  Spring
                     │
        ┌────────────┼─────────────┐
        ↓            ↓             ↓
       IoC           DI        ApplicationContext
```

---

# 31. And THIS is what Spring Boot will eventually hide from you

When you write:

```java
@RestController
@RequestMapping("/users")
public class UserController {

    private final UserService service;

    public UserController(UserService service) {
        this.service = service;
    }

    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return service.getUser(id);
    }
}
```

it looks incredibly simple.

But underneath, there is a lot happening:

```text
HTTP request
     ↓
Tomcat
     ↓
Spring's DispatcherServlet
     ↓
Find matching controller
     ↓
Resolve path variable
     ↓
Call Java method
     ↓
Service bean
     ↓
Repository bean
     ↓
Database
     ↓
Java object
     ↓
Jackson serialization
     ↓
JSON response
```

**Spring is essentially providing the infrastructure that connects all these pieces.**

---

# The mental model I want you to keep

Don't memorize Spring annotations yet. Understand this picture:

```text
                         SPRING CONTAINER
                     ┌──────────────────────┐
                     │                      │
                     │  UserRepository      │
                     │        ↑             │
                     │        │ DI          │
                     │        │             │
                     │   UserService        │
                     │        ↑             │
                     │        │ DI          │
                     │        │             │
                     │   UserController     │
                     │                      │
                     └──────────────────────┘
                              │
                              ↓
                         HTTP / REST
```

And the fundamental transformation is:

### Traditional Java

```text
YOU
 │
 ├── new Repository()
 ├── new Service(repository)
 ├── new Controller(service)
 ├── manage lifecycle
 └── connect everything
```

### Spring

```text
YOU
 │
 └── describe your classes + dependencies
                         ↓
                     SPRING
                         │
             ┌───────────┼───────────┐
             ↓           ↓           ↓
          creates      wires       manages
          objects   dependencies  lifecycle
```

That's **IoC + Dependency Injection**, and those are the foundations upon which the rest of Spring is built.

### Next logical step

After Core, the most useful progression is:

**Spring Core → Spring MVC → REST APIs → Spring Data JPA/Hibernate → PostgreSQL → Spring Security → transactions → testing → Redis → Docker → microservices.**
