## 1. Core Java Fundamentals
- [ ] JVM/JRE/JDK, memory areas (heap, stack, method area)
- [ ] Data types, operators, control flow
- [ ] Strings (immutability, String pool, StringBuilder vs StringBuffer)
- [ ] Wrapper classes, autoboxing/unboxing
- [ ] Exception handling (checked vs unchecked, custom exceptions, try-with-resources)
- [ ] Garbage collection (generations, GC algorithms at a conceptual level)
- [ ] Serialization/deserialization
- [ ] Generics
- [ ] Java 8+ features (lambdas, streams, Optional, functional interfaces) — **heavily interview-tested**

## 2. OOP
- [ ] Four pillars with real examples (not just definitions)
- [ ] Constructors (overloading, chaining, `this`/`super`)
- [ ] Composition vs inheritance — when to use which
- [ ] Design patterns: Singleton, Factory, Builder, Observer, Strategy (enough for LLD rounds, not exhaustive)

## 3. Collections Framework
- [ ] Collection hierarchy (List/Set/Map/Queue)
- [ ] ArrayList vs LinkedList, HashSet vs TreeSet vs LinkedHashSet
- [ ] HashMap internals (hashing, buckets, resize, load factor) — **very commonly asked**
- [ ] Concurrent collections (ConcurrentHashMap, CopyOnWriteArrayList)
- [ ] Comparable vs Comparator

## 4. Multithreading & Concurrency
- [ ] Thread lifecycle, creating threads (Runnable vs Thread)
- [ ] Synchronization (synchronized keyword, locks)
- [ ] java.util.concurrent (ExecutorService, CompletableFuture, atomic classes)
- [ ] Deadlock — causes and prevention

## 5. Spring Boot & Microservices
- [ ] IoC & DI (constructor vs setter vs field injection — know why constructor is preferred)
- [ ] Bean lifecycle and scopes (singleton, prototype, request/session)
- [ ] Core annotations (@Component/@Service/@Repository/@Controller/@RestController, @Autowired, @Qualifier, @Bean, @Configuration)
- [ ] Spring Boot auto-configuration, starters, application.properties/yml
- [ ] Spring Data JPA (repositories, query methods, @Query)
- [ ] Microservices concepts (service boundaries, API gateway, service discovery — conceptual, not building a real multi-service system)
- [ ] Spring Security basics (JWT auth, filters)
- [ ] **Addition: Spring AI basics** — calling LLM APIs from a Spring service, one integration if time allows

## 6. Database & ORM
- [ ] SQL fundamentals (joins, subqueries, indexing, transactions, ACID)
- [ ] JDBC basics
- [ ] JPA/Hibernate (entity mapping, relationships, lazy vs eager loading)
- [ ] N+1 problem and fixes (JOIN FETCH, batch fetching) — **frequently asked**
- [ ] Normalization/denormalization
- [ ] **Addition: Redis** — caching patterns, cache-aside, basic pub/sub

## 7. Testing
- [ ] Unit testing (JUnit, assertions)
- [ ] Mocking vs spying (Mockito)
- [ ] Integration testing basics
- [ ] TDD concept, code coverage awareness

## 8. Git
- [ ] Branching strategies (Git flow, trunk-based)
- [ ] Merge vs rebase, resolving conflicts
- [ ] Basic commands fluency (not just theory — practice actual conflict resolution)

## 9. Agile/Project Management
- [ ] Scrum vs Kanban differences
- [ ] Sprint ceremonies, basic PM tool familiarity (Jira-level understanding)

## 10. Additions Not in the PDF (your gaps)
- [ ] **DSA**: Striver's A-to-Z sheet, 250-300 problems (separate daily track)
- [ ] **LLD**: SOLID principles + classic problems (parking lot, rate limiter, elevator system) — deeper than the PDF's "basic patterns"
- [ ] **HLD**: caching, load balancing, horizontal scaling, CAP theorem, basic distributed systems
- [ ] **Docker**: containerize your flagship project, understand images/volumes/networking basics
- [ ] **AWS**: deploy project (EC2/ECS + RDS + S3), work toward Developer Associate cert; VPC/serverless conceptually
- [ ] **Frontend**: React/Next.js + shadcn/ui — enough to build your flagship project's UI
- [ ] **Flagship project**: one Spring Boot + React/Next.js + Postgres + Redis + Docker app, deployed on AWS, with a feature complex enough to discuss in depth (auth, roles, search/notifications)

## Sequencing note
Sections 1-4 (Core Java/OOP/Collections/Multithreading) → Phase 1. Section 5-6 (Spring/DB) + project start → Phase 2. LLD/HLD/Docker/AWS → Phase 3. Sections 7-9 (Testing/Git/Agile) can be picked up steadily throughout — they're lighter and often folded into project work naturally. DSA runs parallel the whole way through.