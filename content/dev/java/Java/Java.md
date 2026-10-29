# Java Master Checklist

- #interview interview — High-frequency interview topic
- #dsa DSA — Used directly in LeetCode/DSA
- #spring Spring — Spring Boot pre-requisite
- #misc added — Not in original syllabus

---

## [[1. Language Fundamentals & Execution Model]]

- [x] Primitive types vs wrapper classes — memory layout, stack vs heap storage
  - Know exactly where `int` vs `Integer` lives; autoboxing cost in hot paths
- [x] Autoboxing / unboxing & the Integer cache pool (−128 to 127) #interview #misc
  - Classic trap: `Integer a=127; Integer b=127; a==b` is true; `a=128` breaks
- [x] Pass-by-value vs pass-by-reference — the definitive Java answer #interview
  - References are passed by value; mutation visible, reassignment is not
- [x] Type casting — widening vs narrowing, data loss, precision rules #interview
  - `float→int` truncates silently; `long→int` wraps; know the hierarchy
- [x] String pool mechanics — literal vs `new String()`, `intern()`, heap shift in Java 7+ #interview #misc
  - `==` on Strings explained by pool; always be ready to draw this
- [x] String immutability — why Strings are immutable, security and caching implications #interview #misc
  - `hashCode` caching, thread-safety, and why you can't subclass String
- [ ] `StringBuilder` vs `StringBuffer` vs String concatenation performance #interview #misc
  - Loop with `+=` is O(n²); `StringBuilder` is the fix; `StringBuffer` adds sync overhead
- [ ] `equals()` vs `==` — structural vs reference equality, the contract #interview
  - Reflexive, symmetric, transitive, consistent, null-safe
- [ ] `hashCode()` contract, bucket allocation, breaking the contract consequences #interview #dsa
  - If `a.equals(b)` then `a.hashCode()==b.hashCode()` must hold — not the reverse
- [ ] Keywords deep-dive — `final`, `static`, `this`, `super`, `transient`, `volatile`, `strictfp` #interview
  - `final` on a reference ≠ immutable object; static initialisation order matters
- [ ] Variable argument methods (varargs) — syntax, array under the hood, overloading ambiguity #interview #misc
  - Comes up in stream and collection factory method questions
- [ ] Bitwise and shift operators — `&`, `|`, `^`, `~`, `<<`, `>>`, `>>>` #interview #dsa #misc
  - XOR swap, masking, sign extension with `>>` vs unsigned `>>>`; used in HashMap internals AND bit DSA problems

---

## 2. OOP Deep-Dive

- [ ] Encapsulation, inheritance, polymorphism, abstraction — precise definitions with code #interview
  - Not just buzzwords; be able to show each with a non-trivial example
- [ ] Method overloading vs overriding — compile-time vs runtime resolution #interview
  - Static methods are overloaded not overridden — common trick question
- [ ] Runtime polymorphism internals — vtable (virtual method table) and dynamic dispatch #interview #misc
  - How JVM resolves overridden method at runtime; why you can't override static methods
- [ ] Covariant return types and narrowing in overrides #interview #misc
  - Java 5+ allows subtype return; used in builder patterns and cloning
- [ ] Composition vs inheritance — decoupled design, favour composition rule #interview #spring
  - Has-a vs is-a; delegation pattern; Spring beans use composition
- [ ] `instanceof` — pattern matching enhancement (Java 16+) #interview
  - `if(obj instanceof String s)` eliminates the cast; know the old style too
- [ ] `Object` class methods — `equals`, `hashCode`, `toString`, `clone`, `finalize`, `getClass`, `wait`/`notify` #interview
  - All objects inherit these; `finalize` deprecated in Java 9+; know why
- [ ] Object instantiation order — static blocks, instance blocks, constructors, parent chain #interview
  - Classic exam and interview question: what prints when you `new` a subclass?
- [ ] Immutable class recipe — `final` class, `final` fields, defensive copy in constructor AND getter #interview #spring #misc
  - Mutable field (`Date`, `List`) is the trap; copy in, copy out
- [ ] Abstract class vs interface — structural tradeoffs, when to choose which #interview #spring
  - Abstract class: shared state + partial impl; interface: contract + multiple inheritance
- [ ] Default and static methods in interfaces — diamond conflict resolution #interview
  - When two interfaces have same default method, implementing class must override
- [ ] Functional interfaces — Single Abstract Method rule, `@FunctionalInterface` annotation #interview #spring
  - Lambda is syntactic sugar for an anonymous class implementing a functional interface
- [ ] Marker interfaces — `Serializable`, `Cloneable` mechanics; custom marker interfaces #interview
  - `Serializable` has no methods; JVM checks for it via `instanceof`; `serialVersionUID`
- [ ] Java Records — immutable data carriers, auto-generated methods, bytecode difference from POJO #interview #misc
  - `record Point(int x, int y){}` — canonical constructor, accessors, `equals`/`hashCode`/`toString` generated
- [ ] Sealed classes and interfaces — `permits` keyword, exhaustive pattern matching #interview #misc
  - Enables exhaustive switch expressions; restricts class hierarchy intentionally
- [ ] Enum deep-dive — abstract methods per constant, `EnumMap`/`EnumSet` performance, enum as singleton #interview #misc
  - Enum singletons are thread-safe and serialisation-safe by default

---

## 3. JVM Architecture & Memory

- [ ] Stack vs Heap deep-dive — local variable frames, reference allocation, primitive storage, thread isolation #interview
  - Each thread has its own stack; heap is shared; draw this for any GC question
- [ ] Metaspace — replaced PermGen in Java 8, dynamic scaling, class metadata allocation #interview
  - PermGen OOM was common in Java 7; Metaspace grows natively; know the JVM flag
- [ ] ClassLoader subsystem — Bootstrap, Platform, Application loaders; delegation hierarchy #interview #spring
  - Parent-first delegation prevents class hijacking; custom loader use cases
- [ ] Object lifecycle — reachability: strong, soft, weak, and phantom references #interview
  - `WeakHashMap` uses weak refs; `SoftReference` for caches; `PhantomReference` for cleanup
- [ ] Generational GC — Eden, S0/S1 survivor spaces, old/tenured generation, minor vs major GC #interview
  - Most objects die young (weak generational hypothesis); promotion threshold
- [ ] Modern GC collectors — G1GC (default Java 9+), ZGC low-latency, Shenandoah overview #interview
  - G1 divides heap into equal-size regions; ZGC targets sub-millisecond pauses
- [ ] Memory leak patterns in Java — static collections, unclosed streams, ThreadLocal retention, listener accumulation #interview #spring
  - Static `Map` that grows forever; stream not closed in `finally`; `ThreadLocal` in thread pools
- [ ] JVM flags you should know — `-Xms`, `-Xmx`, `-XX:+UseG1GC`, `-XX:MaxMetaspaceSize` #interview #misc
  - Not memorising flags — understanding what they control and why they matter

---

## 4. Exception Handling

- [ ] Exception hierarchy — `Throwable` → `Error` vs `Exception` → `RuntimeException` #interview
  - `Error` is unrecoverable (OOM, StackOverflow); don't catch it in production
- [ ] Checked vs unchecked exceptions — when to use which, architectural implications #interview #spring
  - Checked = recoverable, caller must handle; unchecked = programming error
- [ ] `try-catch-finally` mechanics — execution order, return inside `finally`, JVM exit exception #interview
  - `finally` always runs except `System.exit()`; `return` in `finally` overrides `return` in `try`
- [ ] Try-with-resources — `AutoCloseable` contract, suppressed exceptions, decompiled output #interview
  - Multiple resources in one `try()`; suppressed exceptions stored on primary; order of close
- [ ] Exception chaining — wrapping without losing root cause, `initCause` vs constructor #interview #spring
  - Never swallow the original exception; always pass it as cause
- [ ] Custom exception design — when to extend `Exception` vs `RuntimeException`, meaningful fields #interview #spring
  - Add context fields (`errorCode`, `entityId`); keep message human-readable
- [ ] Multi-catch (Java 7+) and exception union types #interview #misc
  - `catch(IOException | SQLException e)` — `e` is effectively final in this block

---

## 5. Generics & Type Erasure

- [ ] Generic classes, interfaces, and methods — type parameter syntax, naming conventions #interview
  - `T`=type, `E`=element, `K`=key, `V`=value, `R`=return — conventions matter in reviews
- [ ] Bounded wildcards — upper bounds (`? extends T`) vs lower bounds (`? super T`) #interview #dsa
  - `List<? extends Number>` is read-only; `List<? super Integer>` is write-ready
- [ ] PECS principle — Producer Extends, Consumer Super #interview #misc
  - `Collections.copy(dest, src)`: `dest` is `List<? super T>`, `src` is `List<? extends T>`
- [ ] Type erasure — how bytecode strips generics at compile time, bridge methods generated #interview
  - `List<String>` and `List<Integer>` are same class at runtime; can't do `new T()`
- [ ] Generic method inference and target typing #interview #misc
  - Java infers type from context; sometimes you must hint with `<Type>method()`
- [ ] Heap pollution and `@SuppressWarnings("unchecked")` #interview #misc
  - Mixing raw types and generics causes heap pollution; the annotation suppresses, not fixes
- [ ] Reifiable vs non-reifiable types — why you can't create generic arrays #interview #misc
  - `new T[]` illegal because `T` is erased; use `Object[]` with cast or reflection

---

## 6. Collections Framework Internals

- [ ] `ArrayList` — dynamic array, growth factor (1.5×), index-based O(1) get, O(n) insert/delete #interview #dsa
  - `ensureCapacity()` to pre-size; `trimToSize()` to reclaim; `Arrays.copyOf` under the hood
- [ ] `LinkedList` — doubly-linked list, O(1) head/tail add, O(n) index access, memory overhead per node #interview #dsa
  - Use `ArrayDeque` instead of `LinkedList` for queue/deque — less memory, better cache locality
- [ ] `ArrayDeque` — resizable circular array, preferred stack and queue, no null elements #interview #dsa #misc
  - `poll()` vs `pop()`; `offer()` vs `push()`; this is your go-to for BFS/DFS in LeetCode
- [ ] `PriorityQueue` — binary heap, O(log n) offer/poll, O(1) peek, custom `Comparator` for max-heap #interview #dsa #misc
  - `PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder())`
- [ ] `HashMap` internals — `hash()`, `index=(n-1)&hash`, linked list → Red-Black Tree at threshold 8 #interview #dsa
  - Load factor 0.75; resize doubles capacity; treeification and untreeification thresholds
- [ ] `LinkedHashMap` — insertion-order or access-order; how to build an LRU cache with it #interview #dsa #misc
  - Override `removeEldestEntry()` for bounded LRU; access-order mode for eviction
- [ ] `TreeMap` — Red-Black Tree, O(log n) all ops, `NavigableMap` API — `floorKey`, `ceilingKey`, `subMap` #interview #dsa
  - `floorKey(k)` = largest key ≤ k; used in interval, sliding window, and scheduling problems
- [ ] `HashSet` / `LinkedHashSet` / `TreeSet` — backed by respective Maps, `contains()` complexity #interview #dsa
  - `HashSet` is `HashMap<E,PRESENT>`; `contains` is O(1) amortized; `TreeSet` for sorted unique
- [ ] `ConcurrentHashMap` — bucket-level segment locking (Java 8: CAS + synchronized on bin head) #interview #spring
  - `computeIfAbsent` is atomic; iterators are weakly consistent; never use `Hashtable`
- [ ] Fail-fast vs fail-safe iteration — `modCount` tracking vs snapshot copy #interview
  - `ArrayList` iterator throws `ConcurrentModificationException`; `CopyOnWriteArrayList` does not
- [ ] `Comparable` vs `Comparator` — natural ordering vs custom; chaining with `thenComparing()` #interview #dsa
  - `Comparator.comparing(Person::getAge).thenComparing(Person::getName)`
- [ ] Custom `Comparator` for DSA — sorting `int[][]`, multi-key sort, interval problems #interview #dsa #misc
  - `Arrays.sort(intervals, (a,b)->a[0]-b[0])` — careful with overflow; use `Integer.compare()`
- [ ] Unmodifiable vs immutable collections — `Collections.unmodifiableList()` vs `List.of()` #interview
  - `unmodifiableList` wraps mutable; `List.of()` truly immutable and rejects nulls
- [ ] Iterator and Iterable — implementing custom iterator, `hasNext()`/`next()` contract #interview #misc
  - Implement for a custom `LinkedList` or binary tree; removes should call `iterator.remove()`
- [ ] `Collections` utility class — `sort`, `binarySearch`, `frequency`, `disjoint`, `nCopies`, `synchronizedList` #interview #dsa #misc
  - `binarySearch` requires sorted list; returns negative (`-(insertion point)-1`) if absent

---

## 7. Functional Programming & Streams API

- [ ] Lambda expressions — syntax, variable capture rules (effectively final), `this` binding #interview #spring
  - Lambdas capture variables by value; captured vars must be effectively final
- [ ] Built-in functional interfaces — `Predicate`, `Function`, `Consumer`, `Supplier`, `BiFunction`, `UnaryOperator` #interview #spring
  - Know signatures; `compose()` and `andThen()` for chaining `Predicate`s and `Function`s
- [ ] Method references — 4 types: static, instance on arbitrary, instance on specific, constructor #interview #misc
  - `String::valueOf` (static), `str::length` (bound), `String::length` (unbound), `ArrayList::new`
- [ ] Stream pipeline — source, intermediate lazy ops, terminal eager ops; no reuse after terminal #interview #spring
  - Stream is consumed once; intermediate ops build a pipeline, nothing executes until terminal
- [ ] Intermediate operations — `map`, `filter`, `flatMap`, `distinct`, `sorted`, `peek`, `limit`, `skip` #interview #dsa
  - `flatMap` is the key: `Stream<List<T>>` → `Stream<T>`; used heavily in nested collection problems
- [ ] Terminal operations — `collect`, `reduce`, `forEach`, `count`, `min`, `max`, `findFirst`, `anyMatch`, `allMatch` #interview #dsa
  - `reduce(identity, accumulator)`; short-circuit: `findFirst`/`anyMatch` stop early
- [ ] Collectors — `toList`, `toSet`, `toMap`, `groupingBy`, `partitioningBy`, `joining`, `counting`, `summarizingInt` #interview #spring
  - `groupingBy` is most tested: `Map<String, List<Person>>` by department
- [ ] Parallel streams — when to use, thread safety requirements, `ForkJoinPool` internals #interview
  - Shared mutable state = bugs; parallel helps only with large datasets and CPU-bound ops
- [ ] `Optional<T>` — `of` vs `ofNullable` vs `empty`; `map`, `filter`, `flatMap`, `orElse`, `orElseGet`, `orElseThrow` #interview #spring #misc
  - `orElse` always evaluates the argument; `orElseGet` is lazy — critical performance difference
- [ ] Stream vs Collection — when to use each; why not to overuse streams #interview #misc
  - Streams for transformations; collections for random access and mutation

---

## 8. Multithreading & Concurrency

- [ ] Thread lifecycle — NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED #interview
  - `Thread.sleep()` → TIMED_WAITING; `synchronized` → BLOCKED; `wait()` → WAITING
- [ ] `Runnable` vs `Callable<V>` — return value, checked exception support, `Future<V>` #interview
  - `Callable` returns a value; `submit()` returns `Future`; `future.get()` blocks until done
- [ ] Executor framework — `ExecutorService`, `submit` vs `execute`, `shutdown` vs `shutdownNow` #interview #spring
  - Never create raw `Thread`s in production; always use `Executors` or `ThreadPoolExecutor`
- [ ] `ThreadPoolExecutor` parameters — `corePoolSize`, `maxPoolSize`, `keepAlive`, `BlockingQueue` type, `RejectedExecutionHandler` #interview #spring #misc
  - When queue full and max threads busy → rejection; `CallerRunsPolicy` is safest default
- [ ] `synchronized` — monitor lock, method vs block, static sync on class object #interview
  - Intrinsic lock; static `synchronized` locks the `Class` object not the instance
- [ ] `volatile` — memory barrier, prevents compiler reordering, visibility not atomicity #interview
  - `volatile` guarantees all threads see the latest write; doesn't make compound ops atomic
- [ ] Double-checked locking — why `volatile` is mandatory for correct DCL singleton #interview #misc
  - Without `volatile`, partially constructed object can be observed by another thread
- [ ] `ReentrantLock` — explicit lock/unlock, `tryLock` with timeout, fairness, `Condition` variables #interview
  - `ReentrantLock.newCondition()` replaces `wait()`/`notify()`; allows multiple conditions per lock
- [ ] `ReadWriteLock` — concurrent reads, exclusive writes; `StampedLock` optimistic reads #interview #misc
  - Use when reads vastly outnumber writes; `StampedLock` is faster but more complex
- [ ] Atomic classes — `AtomicInteger`, `AtomicLong`, `AtomicReference`; CAS (Compare-And-Swap) internals #interview
  - `compareAndSet(expect, update)` — hardware instruction, no lock; used in lock-free algorithms
- [ ] `ThreadLocal` — per-thread storage, use cases, memory leak in thread pools #interview #misc
  - MDC in logging uses `ThreadLocal`; always call `remove()` in `finally` in pooled threads
- [ ] `CountDownLatch`, `CyclicBarrier`, `Semaphore` — use cases, reset behaviour difference #interview #misc
  - Latch: one-shot; Barrier: resets after each cycle; Semaphore: limits concurrent access count
- [ ] `CompletableFuture` — `thenApply` vs `thenCompose` vs `thenCombine`; `exceptionally` vs `handle` #interview #spring
  - `thenCompose` flattens nested futures; `handle` gets both result and exception
- [ ] Virtual threads (Project Loom) — lightweight threads, structured concurrency, when to use #interview #misc
  - Virtual threads are cheap to create; don't pool them; block freely; Java 21+ GA
- [ ] Common concurrency bugs — deadlock, livelock, race condition, starvation; how to detect #interview #misc
  - Deadlock = circular lock dependency; livelock = threads respond to each other and make no progress

---

## 9. I/O, Serialization & Networking Basics

- [ ] Byte vs character streams — `InputStream`/`OutputStream` vs `Reader`/`Writer` hierarchy #interview
  - `FileInputStream` for binary; `BufferedReader` for text; always buffer for performance
- [ ] `java.nio` — Channels, Buffers, Selectors, non-blocking I/O model #interview
  - `Selector` allows one thread to manage many connections; key to high-throughput servers
- [ ] Serialization — `Serializable` flag, `serialVersionUID` schema versioning, `transient` modifier #interview
  - If `serialVersionUID` doesn't match on deserialize → `InvalidClassException`
- [ ] Java NIO.2 (`java.nio.file`) — `Path`, `Files`, `StandardOpenOption`, directory walking #interview #misc
  - `Files.walk()`; `Files.readAllLines()`; `Files.copy()` — modern file API over legacy `File` class

---

## 10. Reflection, Annotations & Spring Pre-Requisites

- [ ] Reflection API — `Class.forName`, `getDeclaredField`, `setAccessible`, `invoke`; performance cost #interview #spring
  - Breaks encapsulation; Spring uses it for DI wiring; slow — cache `Method`/`Field` references
- [ ] Custom annotation declaration — `@interface`, retention (SOURCE/CLASS/RUNTIME), targets #interview #spring
  - `@Retention(RUNTIME)` needed for reflection-based processing; `@Target` limits where it applies
- [ ] Runtime annotation processing — reading annotations via `getAnnotation()` at runtime #interview #spring
  - This is exactly how `@Autowired`, `@Service`, `@Transactional` work under the hood
- [ ] Dynamic proxies — JDK `Proxy.newProxyInstance()` vs CGLIB byte-buddy proxies #interview #spring #misc
  - Spring uses JDK proxy for interfaces, CGLIB for classes; basis of AOP and `@Transactional`

---

## 11. Design Principles & Patterns

- [ ] SOLID principles — identify violations in code; SRP, OCP, LSP, ISP, DIP #interview #spring
  - LSP violations: subclass breaks superclass contract; ISP: fat interfaces; DIP: depend on abstractions
- [ ] Singleton — double-checked locking with `volatile`, enum singleton (best), holder pattern #interview
  - Enum singleton is serialisation-safe and thread-safe with no extra code
- [ ] Factory & Abstract Factory — hide object creation, decouple client from implementation #interview #spring
  - Spring's `BeanFactory` is literally the Factory pattern at scale
- [ ] Builder — telescoping constructor problem, fluent API, `@Builder` in Lombok #interview #spring
  - Mandatory vs optional fields; call `build()` validation; immutable result
- [ ] Proxy pattern — static proxy vs dynamic proxy; how Spring AOP works #interview #spring #misc
  - `@Transactional` = proxy wraps your method in begin-commit-rollback; understand this deeply
- [ ] Strategy pattern — family of algorithms, interchangeable at runtime #interview #spring
  - `Comparator` is a Strategy; Spring's `ResourceLoader` uses Strategy; replaces if-else chains
- [ ] Template Method — skeleton in abstract class, steps overridden in subclasses #interview #spring
  - `JdbcTemplate`, `RestTemplate` — the template handles boilerplate, you fill the hook method
- [ ] Observer / Event pattern — subject notifies observers; publish-subscribe variant #interview #spring
  - Spring's `ApplicationEventPublisher`; `@EventListener`; decouples producers and consumers
- [ ] Decorator pattern — wrap object to add behaviour without subclassing #interview #misc
  - `BufferedInputStream` wraps `InputStream`; Java I/O is all decorators
- [ ] Dependency Injection pattern — constructor vs setter vs field injection; why constructor preferred #interview #spring #misc
  - Constructor injection: immutable, testable, obvious; field injection hides dependencies

---

## 12. JDBC & Database Connectivity

- [ ] JDBC lifecycle — `DriverManager`, `Connection`, `Statement`/`PreparedStatement`, `ResultSet`, close order #interview #spring
  - Always close `ResultSet` → `Statement` → `Connection`; try-with-resources does this automatically
- [ ] `PreparedStatement` vs `Statement` — SQL injection prevention, execution plan caching #interview #spring
  - `PreparedStatement` compiles once, executes many times; parameterised = safe
- [ ] Transaction isolation levels — READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE #interview #spring
  - Dirty read, non-repeatable read, phantom read — know which level prevents which anomaly
- [ ] Connection pooling — HikariCP configuration, `maxPoolSize`, `connectionTimeout`, leak detection #interview #spring
  - Too small pool = bottleneck; too large = DB overwhelmed; HikariCP is Spring Boot default
- [ ] SQL essentials — JOIN types, GROUP BY + HAVING, subqueries vs CTEs, window functions #interview #spring #misc
  - `ROW_NUMBER() OVER(PARTITION BY dept ORDER BY salary DESC)` — every finance company asks this
- [ ] Database indexing — B-Tree index, composite index, covering index, when index hurts #interview #spring #misc
  - Index on low-cardinality column (boolean) often slower; covering index avoids table fetch
- [ ] N+1 query problem — what it is, how to detect with logging, how to fix with JOIN FETCH #interview #spring #misc
  - Fetching 100 orders then 100 separate queries for each order's items = N+1

---

## 13. Testing

- [ ] JUnit 5 lifecycle — `@BeforeAll`, `@BeforeEach`, `@AfterEach`, `@AfterAll`, `@Nested`, `@ParameterizedTest` #interview #spring
  - `@BeforeAll` method must be static; `@Nested` enables grouped test classes
- [ ] Mockito — `@Mock` vs `@Spy` vs `@InjectMocks`; `when()`, `verify()`, `ArgumentCaptor` #interview #spring
  - Spy wraps real object, partial mock; `ArgumentCaptor` captures what was passed to a mock
- [ ] Test doubles terminology — dummy, fake, stub, spy, mock #interview #misc
  - Stub: canned answers; Mock: verifies interactions; Fake: working lightweight impl (in-memory DB)
- [ ] Testing pyramid — unit → integration → end-to-end; cost and confidence tradeoff #interview #spring #misc
  - Many fast unit tests, fewer slow integration tests; know what each type actually verifies

---

## 14. Build & Tooling

- [ ] Maven lifecycle phases — validate, compile, test, package, verify, install, deploy #interview
  - `mvn package` skips install; `mvn install` puts jar in local `.m2`; know the phase order
- [ ] Maven dependency scopes — compile, provided, runtime, test, system #interview #misc
  - `provided`: on classpath at compile, not packaged (`servlet-api`); `runtime`: not needed to compile
- [ ] Transitive dependency conflicts — nearest-wins, `dependency:tree`, exclusions #interview #misc
  - Two deps pull in different versions of same lib; nearest to root wins; or use `<exclusions>`
- [ ] Gradle basics — `build.gradle` DSL, task lifecycle, `implementation` vs `api` dependency config #interview #misc
  - `api` exposes to consumers; `implementation` hides; Gradle is faster than Maven via caching
- [ ] Git fundamentals — merge vs rebase, cherry-pick, stash, reset vs revert #interview #misc
  - Rebase rewrites history (never on shared branches); revert adds a new commit undoing changes

---

## 15. HTTP, REST & Networking Basics [Spring Boot Pre-Req]

- [ ] HTTP methods — GET, POST, PUT, PATCH, DELETE; idempotency and safety definitions #interview #spring #misc
  - GET/PUT/DELETE are idempotent; POST is not; PATCH partially updates
- [ ] HTTP status codes — 200, 201, 204, 400, 401, 403, 404, 409, 500 #interview #spring #misc
  - 201=created (POST); 204=no content (DELETE); 409=conflict; 401=unauthenticated; 403=unauthorised
- [ ] REST constraints — stateless, uniform interface, resource identification via URI, HATEOAS #interview #spring #misc
  - Stateless means no session on server; each request self-contained; HATEOAS rarely implemented fully
- [ ] Request/response headers — Content-Type, Accept, Authorization, CORS headers #interview #spring #misc
  - `Authorization: Bearer <jwt>`; CORS is browser-enforced, not a server security control
- [ ] JSON basics — serialization/deserialization, Jackson `ObjectMapper`, `@JsonProperty` #interview #spring #misc
  - Jackson is Spring Boot default; `@JsonIgnore`, `@JsonProperty`, `@JsonInclude` are commonly used

---

## 16. System Design Vocabulary [Interview Prep]

- [ ] CAP theorem — Consistency, Availability, Partition tolerance; only 2 of 3; real-world examples #interview #misc
  - CP: MongoDB (default); AP: Cassandra; CA: impossible in distributed system with partitions
- [ ] Horizontal vs vertical scaling — stateless services vs stateful services #interview #misc
  - Horizontal: add nodes; vertical: bigger machine; stateless services scale horizontally easily
- [ ] Caching strategies — cache-aside, write-through, write-behind; eviction policies (LRU, LFU, TTL) #interview #dsa #misc
  - Cache-aside (lazy loading) is most common; LRU eviction via `LinkedHashMap` with access-order
- [ ] Database patterns — read replicas, sharding (horizontal partitioning), connection pooling #interview #misc
  - Sharding splits data by key (`user_id % N`); read replicas handle read-heavy workloads
- [ ] Message queues — async decoupling, at-least-once vs at-most-once vs exactly-once delivery #interview #misc
  - Kafka: at-least-once by default; idempotent consumers needed for exactly-once semantics
- [ ] Load balancing — round-robin, least connections, consistent hashing #interview #misc
  - Consistent hashing minimises reshuffling when nodes join/leave; used in caches and sharding