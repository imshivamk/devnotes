# Java + Spring Boot Backend & Interview Roadmap

# PHASE 0 — ENVIRONMENT & TOOLING SETUP (Pre-Week)

- [x] Install JDK 21 (Temurin) and verify `java -version`.
- [x] Install IntelliJ IDEA Ultimate (or Community + plugins).
- [x] Install Docker Desktop and verify `docker run hello-world`.
- [x] Install Git and configure `user.name`, `user.email`, SSH keys.
- [x] Install Postman / Bruno / Insomnia for API testing.
- [x] Install DBeaver / pgAdmin for database work.
- [x] Set up GitHub account + create `java-backend-roadmap` repo.
- [x] Set up Notion / Obsidian for notes and progress tracking.
- [x] Set up LeetCode + Striver A2Z sheet bookmarks.
- [x] Set up an AI coding assistant (Copilot / Cursor / Claude Code).
- [x] Create a daily time-blocked calendar (DSA, concept, project, review).
- [x] Read "How to prepare for FAANG" style blog posts for calibration.

---

# PHASE 1 — JAVA CORE + DSA FOUNDATION (Months 1–3)

---

## 1. Language Fundamentals & Execution Model

### Primitive Types vs Wrapper Classes `#interview`
- [x] Map stack vs. heap storage for `int` vs `Integer` — draw the memory diagram.
- [x] Trace memory overhead of object headers on wrappers using JOL (Java Object Layout).
- [x] Profile autoboxing/unboxing performance costs in hot loops with JMH or `System.nanoTime()`.
- [x] Write a benchmark comparing `int[]` vs `Integer[]` iteration.
- [x] Explain when to use primitives vs wrappers in production code.

### Autoboxing / Unboxing & The Integer Cache Pool `#interview` `#misc`
- [x] Verify `-128` to `127` pool caching behavior with `==` comparison.
- [x] Debug equality traps: `Integer a = 127; Integer b = 127; a == b` (true) vs `a = 128` (false).
- [x] Inspect JVM flag `-XX:AutoBoxCacheMax` and test with modified values.
- [x] Write a trap: `Integer a = null; int b = a;` → `NullPointerException`.
- [x] Explain the performance impact of autoboxing in collections.
### Pass-By-Value Mechanics `#interview`
- [x] Trace object reference passing by value in stack frames — draw the diagram.
- [x] Demonstrate object state mutation vs. reassignment isolation.
- [x] Write a method that mutates an object's field — show caller sees the change.
- [x] Write a method that reassigns the reference parameter — show caller does NOT see the change.
- [x] Explain why Java is strictly pass-by-value (references are copied).

### Type Casting & Conversions `#interview`
- [x] Trace narrowing truncation (`float` to `int`) — show `3.14f → 3`.
- [x] Trace bit wrapping (`long` to `int`) — show `Long.MAX_VALUE → -1`.
- [x] Verify primitive promotion rules in arithmetic expressions (`byte + byte → int`).
- [x] Write a widening/narrowing conversion hierarchy table.
- [x] Demonstrate implicit vs explicit casting in expressions.

### String Pool Mechanics & Allocation `#interview` `#misc`
- [x] Diagram String pool in Heap (post-Java 7) vs. Metaspace/PermGen.
- [x] Compare string literals vs. explicit `new String()` allocations using `==`.
- [x] Implement and verify `String.intern()` memory deduplication.
- [x] Show `s2.intern() == s1` returns `true` for pooled strings.
- [x] Draw the exact memory layout for `String s = "hello"` vs `new String("hello")`.

### String Immutability `#interview` `#misc`
- [x] Explain security implications (class loading, network sockets, DB credentials).
- [x] Analyze `hashCode()` caching mechanics and thread-safety invariants.
- [x] Trace why `String` is marked `final` in bytecode — show compile error on subclassing.
- [x] List 3 security reasons String is immutable.
- [x] Show the private `hash` field used for caching.

### String Performance & Concatenation `#interview` `#misc`
- [x] Profile O(n²) loop penalty with `+` operator (10,000 iterations).
- [x] Compare `StringBuilder` vs. `StringBuffer` (synchronization overhead).
- [x] Trace default capacity growth factor (2n + 2): 16 → 34 → 70.
- [x] Decompile `"a" + "b" + "c"` to see compiler uses `StringBuilder` or `invokedynamic` (Java 9+).

### Structural vs Reference Equality `#interview`
- [ ] Implement `equals()` satisfying reflexive, symmetric, transitive, consistent contracts.
- [ ] Audit null-safety invariants (`"constant".equals(variable)`).
- [ ] Write `equals()` for a `Person` class with `name` and `age`.
- [ ] Compare `==` vs `.equals()` for Strings, Integers, and custom objects.

### The HashCode Contract `#interview` `#dsa`
- [ ] Enforce consistency between `equals()` and `hashCode()`.
- [ ] Trace hash bucket distribution and collisions.
- [ ] Reproduce map lookup failure caused by mutating keyed object fields.
- [ ] Show `HashMap` lookup failure when `hashCode()` is inconsistent.
- [ ] Trace `index = (n - 1) & hash` for bucket assignment.

### Core Language Keywords `#interview`
- [ ] Distinguish `final` reference reassignment vs. internal object mutation.
- [ ] Sequence static initialization blocks vs. instance blocks vs. constructors.
- [ ] Trace `transient` behavior in serialization.
- [ ] Analyze `volatile` visibility barriers and `strictfp` precision limits.
- [ ] Demonstrate `final` on a reference ≠ immutable object: `final List<String> list = new ArrayList<>(); list.add("x");` works.
- [ ] Show `volatile` flag-based thread stop for visibility.
- [ ] Show `strictfp` precision behavior (default in Java 17+).

### Variable Argument Methods (Varargs) `#interview` `#misc`
- [ ] Trace compiler transformation of `T...` into array instantiations.
- [ ] Resolve method overloading ambiguity edge cases.
- [ ] Write `void printAll(String... args)` and call with 0, 1, and many args.
- [ ] Decompile to show `String[]` array instantiation.
- [ ] Demonstrate overloading ambiguity: `foo(int... a)` vs `foo(int a, int b)`.
- [ ] Show `@SafeVarargs` on a generic varargs method.

### Bitwise & Shift Operators `#interview` `#dsa` `#misc`
- [ ] Implement in-place XOR bit swaps and bitmask extractions.
- [ ] Contrast signed right shift (`>>`) vs. unsigned logical shift (`>>>`).
- [ ] Apply bitwise AND masking (`hash & (n - 1)`) as used in `HashMap`.
- [ ] Implement XOR swap: `a ^= b; b ^= a; a ^= b;`.
- [ ] Extract bits using `(n >> k) & 1`.
- [ ] Contrast `>>` (sign-extending) vs `>>>` (zero-fill) on negative numbers.

### Access Modifiers & Encapsulation Boundaries `#interview`
- [ ] Understand `public`, `protected`, package-private, and `private`.
- [ ] Understand package-level access and how packages affect encapsulation.
- [ ] Distinguish access control from inheritance and overriding rules.
- [ ] Write a class with all four modifiers and access each from: same class, same package, subclass, unrelated class.
- [ ] Draw a visibility table.

### Packages, Imports & Compilation Units `#interview` `#misc`
- [ ] Understand package declarations, imports, fully-qualified names, and static imports.
- [ ] Understand how Java source files map to packages and compiled classes.
- [ ] Create a package `com.example.app` and compile it.
- [ ] Use `import static java.lang.Math.*;` and call `max(1, 2)`.
- [ ] Show fully-qualified name usage without import.
- [ ] Explain how `.java` maps to `.class` file path.

### Constructors, `this` & `super` `#interview`
- [ ] Trace constructor chaining using `this()` and `super()`.
- [ ] Understand constructor inheritance rules and initialization order.
- [ ] Write constructor chaining with `this()` and `super()`.
- [ ] Trace output of a parent-child constructor chain.
- [ ] Show constructors are not inherited.

### Scope, Lifetime & Variable Categories `#interview`
- [ ] Distinguish local variables, instance fields, static fields, parameters, and block scope.
- [ ] Relate variable lifetime to stack frames, objects, and class lifetime.
- [ ] Write examples of each variable category.
- [ ] Draw stack frame with local variables.
- [ ] Show static variable lifetime = class lifetime.

### Date & Time API `#interview` `#spring`
- [ ] Use `LocalDate`, `LocalDateTime`, `Instant`, `Duration`, `Period`, and `ZoneId`.
- [ ] Understand why `java.time` is preferred over legacy `Date`/`Calendar`.
- [ ] Handle timezone-aware timestamps correctly in backend applications.
- [ ] Convert `Instant` to `ZonedDateTime` in two zones.
- [ ] Parse and format an ISO timestamp.

### BigDecimal & Numeric Precision `#interview` `#spring`
- [ ] Understand floating-point precision limitations.
- [ ] Use `BigDecimal` for money and other exact decimal calculations.
- [ ] Understand scale, rounding modes, and safe construction from numeric values.
- [ ] Show `0.1 + 0.2 != 0.3` with `double`.
- [ ] Demonstrate `BigDecimal.valueOf(0.1)` vs `new BigDecimal(0.1)`.
- [ ] Set scale and rounding mode: `setScale(2, RoundingMode.HALF_UP)`.

### Internationalization (i18n) & Localization `#spring` `#misc`
- [ ] Use `ResourceBundle` and `Locale` for message resolution.
- [ ] Configure Spring `MessageSource` for i18n error messages.
- [ ] Handle locale-aware date, number, and currency formatting.
- [ ] Understand timezone-aware storage (UTC) vs display (local).

### Currency & Money Handling in APIs `#spring` `#misc`
- [ ] Use `BigDecimal` + `Currency` type for monetary values.
- [ ] Store money as minor units (paise, cents) as `long` for precision.
- [ ] Handle multi-currency conversions and exchange rate sources.
- [ ] Validate rounding rules per currency.

---

## 2. Object-Oriented Programming & Low-Level Design (LLD)

### Core OOP Pillars in Production Code `#interview`
- [ ] Implement Encapsulation via defensive copying and immutable access boundaries.
- [ ] Implement Polymorphism via unified domain interfaces (e.g., payment gateways).
- [ ] Implement Abstraction separating interface contracts from execution logic.
- [ ] Show each pillar with a non-trivial production-style example.

### Method Overloading vs Overriding `#interview`
- [ ] Trace compile-time static binding vs. runtime dynamic dispatch.
- [ ] Demonstrate static method shadowing/hiding vs. overriding.
- [ ] Write overloaded methods resolved at compile time.
- [ ] Write overridden methods resolved at runtime.
- [ ] Show static methods are overloaded, not overridden.

### Runtime Polymorphism Internals `#interview` `#misc`
- [ ] Diagram Virtual Method Table (vtable) lookups in the JVM.
- [ ] Explain why `private`, `static`, and `final` methods bypass vtables.
- [ ] Show `invokevirtual` vs `invokestatic` in bytecode.

### Covariant Return Types `#interview` `#misc`
- [ ] Implement method overriding returning narrowed subclass types.
- [ ] Apply covariant return types in fluent builder implementations.
- [ ] Show Java 5+ allows subtype return.

### Composition vs Inheritance `#interview` `#spring`
- [ ] Identify fragile base class anti-patterns.
- [ ] Refactor inheritance hierarchies into composition via the Delegation Pattern.
- [ ] Show `has-a` vs `is-a` with code.

### Instanceof Pattern Matching (Java 16+) `#interview`
- [ ] Replace explicit casting blocks with `if (obj instanceof String s)`.
- [ ] Apply pattern matching guards inside switch expressions.
- [ ] Show the old style too.

### Object Base Methods `#interview`
- [ ] Override `toString()`, `clone()`, and `getClass()`.
- [ ] Evaluate why `finalize()` was deprecated and removed.
- [ ] Trace inter-thread signaling with `wait()`, `notify()`, and `notifyAll()`.
- [ ] Show all objects inherit these methods.

### Object Instantiation Sequence `#interview`
- [ ] Step through execution: Parent static → Child static → Parent instance → Parent constructor → Child instance → Child constructor.
- [ ] Write a parent-child class with static and instance blocks.
- [ ] Trace the print output.

### Immutable Class Design Recipe `#interview` `#spring` `#misc`
- [ ] Mark class `final` and all fields `private final`.
- [ ] Perform defensive copies in constructors for mutable fields (e.g., `Date`, `List`).
- [ ] Return unmodifiable views or cloned instances from getter methods.
- [ ] Show mutable field (`Date`, `List`) is the trap; copy in, copy out.

### Abstract Classes vs Interfaces `#interview` `#spring`
- [ ] Evaluate tradeoffs: shared mutable state vs. multiple contract inheritance.
- [ ] Resolve Diamond Problem conflicts in multiple interface default methods.
- [ ] Write an abstract class with shared state.
- [ ] Write an interface with default methods.
- [ ] Resolve diamond conflict with explicit override.

### Functional Interfaces `#interview` `#spring`
- [ ] Validate Single Abstract Method (SAM) rule with `@FunctionalInterface`.
- [ ] Contrast lambda implementation mechanics against anonymous inner classes.
- [ ] Compare lambda vs anonymous inner class bytecode.

### Marker Interfaces `#interview`
- [ ] Inspect JVM runtime type-checking for `Serializable` and `Cloneable`.
- [ ] Create custom marker interfaces for domain processing pipelines.
- [ ] Show `Serializable` has no methods; JVM checks via `instanceof`.

### Java Records `#interview` `#misc`
- [ ] Construct data-carrier records using `record Point(int x, int y) {}`.
- [ ] Customize canonical and compact record constructors.
- [ ] Inspect generated bytecode for immutability and final fields.
- [ ] Show auto-generated `equals`/`hashCode`/`toString`.

### Sealed Classes & Interfaces `#interview` `#misc`
- [ ] Define restricted class trees using `sealed`, `permits`, `non-sealed`, and `final`.
- [ ] Implement exhaustive compiler-checked switch expressions without default branches.
- [ ] Define `sealed interface Shape permits Circle, Square {}`.

### Enum Deep-Dive `#interview` `#misc`
- [ ] Implement constant-specific abstract methods within enums.
- [ ] Benchmark `EnumMap` and `EnumSet` bit-vector performance.
- [ ] Construct thread-safe, serialization-safe singletons via enums.

### SOLID Principles Implementation `#interview` `#spring`
- [ ] Single Responsibility: Decouple business domains from validation and persistence.
- [ ] Open/Closed: Extend processing logic via Strategy pattern interfaces.
- [ ] Liskov Substitution: Verify derived classes never throw unexpected contract exceptions.
- [ ] Interface Segregation: Split bloated contracts into lean client-specific interfaces.
- [ ] Dependency Inversion: Inject service interfaces instead of concrete classes.

### Low-Level Design (LLD) Problem Sets `#interview` `#dsa`
- [ ] **Parking Lot System**
  - [ ] Design multi-floor slot allocation algorithms (Compact, Large, EV).
  - [ ] Implement polymorphic ticketing and dynamic fee calculation strategies.
  - [ ] Ensure thread-safe slot reservation under concurrent entry points.
- [ ] **Rate Limiter**
  - [ ] Implement the Token Bucket algorithm using atomic counters.
  - [ ] Implement the Leaky Bucket algorithm with queue buffers.
  - [ ] Implement the Sliding Window Log algorithm to prevent boundary spikes.
- [ ] **Elevator Control System**
  - [ ] Model internal elevator state transitions (Idle, Moving Up, Moving Down).
  - [ ] Implement SCAN/LOOK request dispatching algorithms.
  - [ ] Manage concurrent pickup/dropoff requests across multiple elevator cars.
- [ ] **Additional LLD Problems** (add as you progress)
  - [ ] Library Management System.
  - [ ] ATM / Banking System.
  - [ ] Chess Game.
  - [ ] Snake & Ladder.
  - [ ] Cab Booking (Uber-like).
  - [ ] Food Delivery (Swiggy-like).
  - [ ] Splitwise.
  - [ ] Cache (LRU/LFU).

---

## 3. JVM Architecture & Memory Management

### Stack vs Heap Internals `#interview`
- [ ] Diagram thread-isolated stack frames (local variable table, operand stack, frame data).
- [ ] Trace heap allocation regions and analyze thread contention.
- [ ] Diagnose `StackOverflowError` vs. `OutOfMemoryError: Java heap space`.

### Metaspace Mechanics `#interview`
- [ ] Trace dynamic off-heap native class metadata storage.
- [ ] Monitor Metaspace expansion and tune `-XX:MaxMetaspaceSize`.
- [ ] Diagnose classloader leak-induced `OutOfMemoryError: Metaspace`.

### ClassLoader Subsystem `#interview` `#spring`
- [ ] Trace parent-first delegation across Bootstrap, Platform, and Application ClassLoaders.
- [ ] Break delegation order using custom ClassLoaders for plugin architectures.

### Object Lifecycle & Reference Reachability `#interview`
- [ ] Trace Strong references and Garbage Collection root path traversal.
- [ ] Implement in-memory caches with `SoftReference` for heap-sensitive data.
- [ ] Use `WeakHashMap` with `WeakReference` keys to prevent memory retention.
- [ ] Coordinate `PhantomReference` with `ReferenceQueue` for safe resource cleanup.

### Generational Garbage Collection `#interview`
- [ ] Trace object allocations in Eden and migrations through Survivor spaces (S0/S1).
- [ ] Calculate object age thresholds for Tenured/Old generation promotion.
- [ ] Differentiate Minor GC, Major GC, and Full GC impact.

### Production GC Collectors `#interview`
- [ ] Tune G1GC region-based partitioning, humongous objects, and pause target flags.
- [ ] Profile ZGC colored pointers and load barriers for sub-millisecond pauses.
- [ ] Contrast Shenandoah concurrent evacuation with G1GC mechanics.
- [ ] Understand Generational ZGC (Java 21+).

### Memory Leak Patterns & Detection `#interview` `#spring`
- [ ] Reproduce memory leaks from unbounded static collections.
- [ ] Track unclosed I/O streams and unreleased database connections.
- [ ] Clear unremoved `ThreadLocal` variables in pooled container threads.
- [ ] Analyze heap dumps (`.hprof`) via Eclipse MAT or JProfiler.

### Essential JVM Tuning Flags `#interview` `#misc`
- [ ] Configure `-Xms` and `-Xmx` to identical bounds to eliminate resizing pauses.
- [ ] Apply `-XX:+UseG1GC`, `-XX:+HeapDumpOnOutOfMemoryError`, and `-XX:HeapDumpPath`.

---

## 4. Exception Handling

### Exception Hierarchy Architecture `#interview`
- [ ] Map hierarchy: `Throwable` → `Error` vs. `Exception` → `RuntimeException`.
- [ ] Audit application code to ensure fatal `Error` types are never caught.

### Checked vs Unchecked Strategy `#interview` `#spring`
- [ ] Apply checked exceptions for recoverable conditions requiring caller intervention.
- [ ] Apply unchecked runtime exceptions for programmatic defects and contract breaches.

### Execution Order & Finally Blocks `#interview`
- [ ] Trace return statement precedence inside `try`, `catch`, and `finally`.
- [ ] Verify execution blocks where `finally` is suppressed (e.g., `System.exit()`).

### Try-With-Resources (ARM) `#interview`
- [ ] Implement the `AutoCloseable` interface on custom resources.
- [ ] Inspect reverse closure order of declared resources.
- [ ] Retrieve suppressed exceptions using `Throwable.getSuppressed()`.

### Exception Chaining `#interview` `#spring`
- [ ] Wrap low-level SQL/IO exceptions inside high-level domain exceptions.
- [ ] Preserve root cause stack traces using `initCause()` and cause-accepting constructors.

### Custom Exception Design `#interview` `#spring`
- [ ] Build contextual exceptions containing error codes, resource IDs, and timestamps.
- [ ] Extend `RuntimeException` for clean propagation to global REST advice handlers.

### Multi-Catch & Union Types `#interview` `#misc`
- [ ] Consolidate identical catch logic using `catch (IOException | SQLException e)`.
- [ ] Verify that multi-catch reference variables are implicitly `final`.

---

## 5. Generics & Type Erasure

### Generic Classes, Interfaces, and Methods `#interview`
- [ ] Write parameterized classes, bounded interfaces, and standalone generic methods.
- [ ] Apply standard naming conventions (`T`, `E`, `K`, `V`, `U`).

### Bounded Wildcards `#interview` `#dsa`
- [ ] Enforce upper bounds (`? extends T`) for read-only producer collections.
- [ ] Enforce lower bounds (`? super T`) for write-enabled consumer collections.

### The PECS Principle `#interview` `#misc`
- [ ] Implement collection utility methods using Producer-Extends, Consumer-Super.
- [ ] Analyze the contract behind `Collections.copy(List<? super T> dest, List<? extends T> src)`.

### Type Erasure Under The Hood `#interview`
- [ ] Decompile generics into bytecode to inspect bounding type replacements.
- [ ] Inspect synthetic bridge methods generated to preserve polymorphism.

### Generic Method Inference `#interview` `#misc`
- [ ] Trace compiler type inference in nested generic invocations.
- [ ] Apply explicit type witness syntax (`Collections.<String>emptyList()`).

### Heap Pollution & Compiler Warnings `#interview` `#misc`
- [ ] Reproduce heap pollution by mixing raw types with parameterized generics.
- [ ] Evaluate safe usage of `@SuppressWarnings("unchecked")` and `@SafeVarargs`.

### Reifiable vs Non-Reifiable Types `#interview` `#misc`
- [ ] Analyze compile failure when attempting array creation of generic types (`new T[]`).
- [ ] Implement generic arrays safely using `Array.newInstance()` or explicit casts.

---

## 6. Collections Framework Internals

### ArrayList Architecture `#interview` `#dsa`
- [ ] Trace internal `Object[] elementData` dynamic resizing by 1.5×.
- [ ] Optimize mass additions using `ensureCapacity()` to minimize array copies.
- [ ] Calculate time complexities: O(1) amortized append, O(n) positional insertion/deletion.

### LinkedList Realities `#interview` `#dsa`
- [ ] Trace doubly-linked `Node<E>` pointers (`prev`, `next`, `item`).
- [ ] Measure cache misses and memory overhead compared to continuous arrays.

### ArrayDeque as Preferred Buffer `#interview` `#dsa` `#misc`
- [ ] Trace circular array head/tail pointer wrapping using bitwise masks.
- [ ] Use `ArrayDeque` instead of `Stack` and `LinkedList` for LIFO/FIFO operations.

### PriorityQueue Internals `#interview` `#dsa` `#misc`
- [ ] Trace array-based binary min-heap representation and bubble-up/down logic.
- [ ] Configure custom `Comparator` instances for max-heaps and multi-field sorting.

### HashMap Internals `#interview` `#dsa`
- [ ] Trace perturbation function: `(h = key.hashCode()) ^ (h >>> 16)`.
- [ ] Map bucket index via bitwise masking: `index = (n - 1) & hash`.
- [ ] Trace treeification (threshold 8, min table capacity 64) and untreeification (threshold 6).
- [ ] Trace resizing: 2× capacity expansion, rehashing, and table transfers.

### LinkedHashMap Mechanics `#interview` `#dsa` `#misc`
- [ ] Trace doubly-linked list ordering superimposed across hash buckets.
- [ ] Implement a bounded LRU cache by overriding `removeEldestEntry()`.

### TreeMap & NavigableMap `#interview` `#dsa`
- [ ] Trace Red-Black tree self-balancing rotations and color-flip invariants.
- [ ] Query boundaries using `floorKey()`, `ceilingKey()`, `higherEntry()`, and `subMap()`.

### Set Implementations `#interview` `#dsa`
- [ ] Map `HashSet` operations directly to backing `HashMap` dummy values.
- [ ] Maintain deterministic iteration order with `LinkedHashSet`.
- [ ] Enforce logarithmic sorted unique invariants with `TreeSet`.

### ConcurrentHashMap Architecture `#interview` `#spring`
- [ ] Trace bucket-level synchronization: CAS for empty nodes, `synchronized` on bin heads.
- [ ] Perform thread-safe modifications using `computeIfAbsent()`.
- [ ] Iterate safely using weakly-consistent, non-blocking iterators.

### Fail-Fast vs Fail-Safe Iteration `#interview`
- [ ] Trace structural mutation tracking with internal `modCount` counters.
- [ ] Handle `ConcurrentModificationException` risks.
- [ ] Analyze snapshot iteration in `CopyOnWriteArrayList` and its write-penalty costs.

### Comparable vs Comparator `#interview` `#dsa`
- [ ] Implement natural domain ordering with `Comparable<T>`.
- [ ] Compose multi-tier sorting pipelines using `Comparator.comparing().thenComparing()`.

### Custom Comparators for DSA `#interview` `#dsa` `#misc`
- [ ] Prevent integer subtraction overflow bugs using `Integer.compare(a, b)`.
- [ ] Sort multi-dimensional primitive arrays (`int[][]`) for geometric and interval logic.

### Unmodifiable vs Immutable Collections `#interview`
- [ ] Inspect read-only wrapper views (`Collections.unmodifiableList()`) against backing mutation.
- [ ] Construct deeply immutable collections using `List.of()`, `Set.of()`, and `Map.of()`.

### Custom Iterators `#interview` `#misc`
- [ ] Implement `Iterable<T>` and `Iterator<T>` on custom tree and graph structures.
- [ ] Enforce the `hasNext()`, `next()`, and safe `iterator.remove()` contract.

### Sequenced Collections (Java 21+) `#interview` `#misc`
- [ ] Understand `SequencedCollection`, `SequencedSet`, `SequencedMap` interfaces.
- [ ] Use `getFirst()`, `getLast()`, `addFirst()`, `addLast()`, `reversed()`.
- [ ] Apply to `List`, `Deque`, `LinkedHashMap`, `SortedMap`.

---

## 7. Functional Programming & Streams API

### Lambda Expressions & Scope Capture `#interview` `#spring`
- [ ] Enforce effectively final constraints on local captured variables.
- [ ] Resolve lexical scope and `this` reference binding inside lambdas.

### Core Functional Interfaces `#interview` `#spring`
- [ ] Implement standard functional primitives: `Predicate<T>`, `Function<T, R>`, `Consumer<T>`, `Supplier<T>`.
- [ ] Compose functional chains using `andThen()`, `compose()`, and `and()`.

### Method References `#interview` `#misc`
- [ ] Static method references (`Math::max`).
- [ ] Bound instance method references (`System.out::println`).
- [ ] Unbound instance method references (`String::toUpperCase`).
- [ ] Constructor references (`ArrayList::new`).

### Stream Pipeline Mechanics `#interview` `#spring`
- [ ] Trace lazy evaluation of intermediate operations until terminal execution.
- [ ] Handle `IllegalStateException` on attempts to consume closed streams.

### Intermediate Stream Operations `#interview` `#dsa`
- [ ] Transform data using `map()`, `filter()`, and `peek()`.
- [ ] Flatten nested data structures using `flatMap()`.
- [ ] Handle stateful intermediate overheads in `sorted()` and `distinct()`.

### Terminal Stream Operations `#interview` `#dsa`
- [ ] Aggregate values using `reduce(identity, accumulator, combiner)`.
- [ ] Apply short-circuit terminal operations (`findFirst()`, `anyMatch()`, `allMatch()`).

### Advanced Collectors `#interview` `#spring`
- [ ] Partition and group data structures using `Collectors.groupingBy()`.
- [ ] Aggregate complex summaries using `Collectors.toMap()`, `joining()`, and `summarizingInt()`.

### Parallel Stream Processing `#interview`
- [ ] Profile thread distribution over the shared `ForkJoinPool.commonPool()`.
- [ ] Eliminate race conditions from shared mutable state in parallel streams.

### Optional Deep-Dive `#interview` `#spring` `#misc`
- [ ] Compare eager evaluation of `orElse()` against lazy evaluation of `orElseGet()`.
- [ ] Chain transforms using `Optional.map()`, `flatMap()`, and `filter()`.
- [ ] Replace anti-pattern `optional.get()` with `orElseThrow()`.

---

## 8. Multithreading & High Concurrency

### Thread Lifecycle & State Transitions `#interview`
- [ ] Map lifecycle states: `NEW`, `RUNNABLE`, `BLOCKED`, `WAITING`, `TIMED_WAITING`, `TERMINATED`.
- [ ] Distinguish monitor contention (`BLOCKED`) from conditional waiting (`WAITING`).

### Runnable vs Callable Execution `#interview`
- [ ] Wrap tasks returning results and throwing checked exceptions with `Callable<V>`.
- [ ] Manage asynchronous execution and non-blocking checks with `Future<V>`.

### Executor Framework & Thread Pools `#interview` `#spring`
- [ ] Manage thread pool lifecycles via `ExecutorService`.
- [ ] Contrast graceful draining (`shutdown()`) with immediate interruption (`shutdownNow()`).

### ThreadPoolExecutor Tuning `#interview` `#spring` `#misc`
- [ ] Configure `corePoolSize`, `maximumPoolSize`, and `keepAliveTime`.
- [ ] Select appropriate work queues: `ArrayBlockingQueue`, `LinkedBlockingQueue`, `SynchronousQueue`.
- [ ] Handle saturation with rejection policies: `AbortPolicy`, `CallerRunsPolicy`, `DiscardOldestPolicy`.

### Thread Pool Sizing Math & Little's Law `#interview` `#misc`
- [ ] Apply Little's Law: `L = λ × W` (concurrency = throughput × latency).
- [ ] Size CPU-bound pools: `N_cpu + 1`.
- [ ] Size I/O-bound pools: `N_cpu × (1 + wait_time / service_time)`.
- [ ] Explain why `maximumPoolSize` is rarely the answer; queue behavior dominates.
- [ ] Understand virtual threads eliminate pool sizing for blocking I/O.

### Intrinsic Locking (Synchronized) `#interview`
- [ ] Trace object header Mark Word lock inflation (Biased → Lightweight → Heavyweight).
- [ ] Contrast instance synchronization locks with static class-level monitors.

### Volatile & The Java Memory Model (JMM) `#interview`
- [ ] Enforce visibility across CPU caches via memory barriers.
- [ ] Prevent instruction reordering optimizations by the compiler and CPU.
- [ ] Demonstrate non-atomicity of compound operations (`count++`) on volatile fields.

### Double-Checked Locking Singleton `#interview` `#misc`
- [ ] Implement thread-safe DCL using `volatile` to prevent partially initialized reads.
- [ ] Alternative: Implement the Bill Pugh Static Helper Holder pattern.

### Explicit Locks (ReentrantLock) `#interview`
- [ ] Implement non-blocking locks with `tryLock()` and acquisition timeouts.
- [ ] Configure lock fairness policies and manage explicit `lock()` / `unlock()` blocks.
- [ ] Coordinate thread signaling with multiple `Condition` variables (`await()`, `signal()`).

### ReadWriteLock & StampedLock `#interview` `#misc`
- [ ] Maximize read throughput on read-heavy structures with `ReentrantReadWriteLock`.
- [ ] Implement optimistic read validations with `StampedLock` to eliminate write starvation.

### Atomic Variables & Lock-Free Programming `#interview`
- [ ] Trace Compare-And-Swap (CAS) hardware instructions (`AtomicInteger`, `AtomicReference`).
- [ ] Implement optimistic lock-free loops using `compareAndSet()`.

### ThreadLocal Storage `#interview` `#misc`
- [ ] Isolate execution context per thread (e.g., MDC logging, transaction contexts).
- [ ] Eliminate thread pool memory leaks by executing `ThreadLocal.remove()` in `finally` blocks.

### Synchronization Utilities `#interview` `#misc`
- [ ] Implement one-shot countdown latches using `CountDownLatch`.
- [ ] Synchronize cyclical rendezvous phases using `CyclicBarrier`.
- [ ] Throttle resource access and rate limits using `Semaphore`.

### Asynchronous Pipelines (CompletableFuture) `#interview` `#spring`
- [ ] Chain dependencies using `thenApply()`, `thenCompose()`, and `thenCombine()`.
- [ ] Handle errors asynchronously using `exceptionally()` and `handle()`.
- [ ] Coordinate multiple async operations using `CompletableFuture.allOf()`.

### Virtual Threads (Project Loom - Java 21+) `#interview` `#misc`
- [ ] Mount lightweight virtual threads to underlying carrier OS worker threads.
- [ ] Eliminate thread pool wrapping for virtual thread instances.
- [ ] Benchmark high-throughput blocking I/O against traditional thread architectures.

### Structured Concurrency (Preview) `#interview` `#misc`
- [ ] Understand `StructuredTaskScope` for fork-join of related tasks.
- [ ] Compare with `CompletableFuture` and manual `ExecutorService`.
- [ ] Handle cancellation and error propagation.

### Scoped Values (Preview, replaces ThreadLocal) `#interview` `#misc`
- [ ] Understand `ScopedValue` for immutable, inheritable context.
- [ ] Compare with `ThreadLocal` for virtual threads.

### Concurrency Anti-Patterns & Diagnostics `#interview` `#misc`
- [ ] Trace Coffman conditions leading to thread deadlocks.
- [ ] Diagnose lock contention and live deadlocks using `jcmd`, `jstack`, and visual profilers.
- [ ] Debug livelock, priority inversion, and thread starvation scenarios.

---

## 9. I/O, Serialization & Networking Basics

### Stream I/O Architecture `#interview`
- [ ] Contrast byte streams (`InputStream`/`OutputStream`) vs. character streams (`Reader`/`Writer`).
- [ ] Wrap low-level streams in `BufferedInputStream` and `BufferedReader` for performance.

### Non-Blocking I/O (java.nio) `#interview`
- [ ] Manage data flow across `Channels`, `Buffers` (capacity, position, limit), and `Selectors`.
- [ ] Build single-threaded multiplexed servers handling concurrent open sockets.

### Java Native Serialization `#interview`
- [ ] Implement `Serializable` and manage schema versioning with `serialVersionUID`.
- [ ] Suppress sensitive field persistence using the `transient` modifier.
- [ ] Identify security vulnerabilities inherent in native Java deserialization.

### Modern File System NIO.2 `#interview` `#misc`
- [ ] Manipulate file system paths safely using `Path` and `Files`.
- [ ] Traverse deeply nested directory structures using `Files.walk()` streams.

### File Uploads & Multipart `#spring` `#misc`
- [ ] Implement multipart file upload in Spring (`MultipartFile`).
- [ ] Stream large files to S3 without loading into memory.
- [ ] Validate file type, size, and content.
- [ ] Integrate virus scanning (ClamAV) in the upload pipeline.

---

## 10. HTTP, REST & Web Fundamentals

### HTTP Request/Response Model `#interview` `#spring`
- [ ] Trace request line, headers, body, response status line, headers, and body.
- [ ] Understand `Content-Type` vs. `Accept`.
- [ ] Understand cookies, `Authorization`, caching headers, and connection keep-alive.

### HTTP Methods & Semantics `#interview` `#spring`
- [ ] Understand GET, POST, PUT, PATCH, DELETE, HEAD, and OPTIONS.
- [ ] Distinguish safe methods from idempotent methods.
- [ ] Understand when PUT vs. PATCH is appropriate.

### HTTP Status Codes & Error Semantics `#interview` `#spring`
- [ ] Understand 2xx, 3xx, 4xx, and 5xx categories.
- [ ] Distinguish 400, 401, 403, 404, 409, 422, 429, and 500 conceptually.

### HTTP/2 and HTTP/3 `#interview` `#misc`
- [ ] Understand multiplexing, header compression, server push (HTTP/2).
- [ ] Understand QUIC transport for HTTP/3.
- [ ] Know when they matter for backend APIs.

### HTTPS & TLS Basics `#interview` `#misc`
- [ ] Understand what TLS provides: encryption, integrity, and server authentication.
- [ ] Understand certificates and the high-level TLS handshake.
- [ ] Understand mTLS for service-to-service authentication.

### TCP/IP Networking Fundamentals `#interview`
- [ ] Understand IP addresses, ports, sockets, TCP vs. UDP, and packet delivery at a high level.
- [ ] Trace the TCP three-way handshake and connection termination conceptually.
- [ ] Understand latency, bandwidth, packet loss, and why network calls are expensive.

### DNS Fundamentals `#interview` `#misc`
- [ ] Understand domain names, DNS resolution, records, and caching.
- [ ] Trace a high-level path from a browser/application to a remote server.

### REST API Design Fundamentals `#interview` `#spring`
- [ ] Model APIs around resources and representations.
- [ ] Design CRUD endpoints using appropriate HTTP methods and status codes.
- [ ] Implement pagination, filtering, sorting, and basic API versioning.
- [ ] Understand DTOs and why API contracts should not blindly expose persistence entities.

### Cookies, Sessions & Statelessness `#interview` `#spring`
- [ ] Understand session-based authentication vs. stateless token-based authentication.
- [ ] Understand cookie attributes such as `HttpOnly`, `Secure`, and `SameSite`.

### JSON & Jackson Fundamentals `#interview` `#spring`
- [ ] Serialize Java objects/records into JSON and deserialize JSON into Java objects.
- [ ] Understand Jackson `ObjectMapper`, nested objects, collections, dates, null handling, and annotations.
- [ ] Understand the difference between serialization and deserialization.

### API Versioning & Contract Testing `#interview` `#spring`
- [ ] Compare URI versioning vs header versioning vs content negotiation.
- [ ] Design OpenAPI / Swagger spec-first APIs.
- [ ] Implement Contract Testing with Pact.
- [ ] Apply Consumer-Driven Contracts.
- [ ] Plan API deprecation and sunset policies.

### GraphQL `#interview` `#spring`
- [ ] Understand schema-first vs code-first approaches.
- [ ] Implement queries, mutations, and subscriptions.
- [ ] Handle N+1 with DataLoader.
- [ ] Set up Spring for GraphQL basics.
- [ ] Compare GraphQL vs REST trade-offs.

### gRPC & Protocol Buffers `#interview` `#spring` `#misc`
- [ ] Define Protobuf schema and understand evolution rules.
- [ ] Implement unary, server-streaming, client-streaming, bidirectional.
- [ ] Compare gRPC vs REST vs GraphQL.
- [ ] Set up Spring gRPC / grpc-java basics.
- [ ] Understand deadlines, cancellation, interceptors.

---

## 11. Reflection, Annotations & Framework Foundations

### Reflection API Internals `#interview` `#spring`
- [ ] Inspect classes, methods, and private fields via `Class<?>` metadata.
- [ ] Bypass access modifiers with `field.setAccessible(true)`.
- [ ] Measure JIT optimization penalties and reflection invocation overhead.

### Custom Annotation Architecture `#interview` `#spring`
- [ ] Build custom annotations using `@Retention(RetentionPolicy.RUNTIME)` and `@Target`.
- [ ] Process custom annotation metadata dynamically at runtime.

### Dynamic Proxies & Bytecode Generation `#interview` `#spring` `#misc`
- [ ] Implement interface-based dynamic proxies via `Proxy.newProxyInstance()`.
- [ ] Compare JDK dynamic proxies with class-intercepting CGLIB / ByteBuddy proxies.
- [ ] Trace runtime proxy generation used in Spring `@Transactional` and AOP.

---

## 12. DSA TRACK — Daily Practice (Months 1–12)

### Striver's A-to-Z DSA Target (250–300 Problems, 2–3 Daily) `#dsa`

#### Arrays & Hashing
- [ ] Two Pointers (pair sum, remove duplicates, container with most water).
- [ ] Prefix Sums (subarray sum equals K, range sum query).
- [ ] Kadane's Algorithm (max subarray sum, max product subarray).
- [ ] Moore's Voting (majority element).
- [ ] Hashing (two sum, group anagrams, longest consecutive sequence).

#### Two Pointers & Sliding Window
- [ ] Fixed-size window (max sum subarray of size K).
- [ ] Dynamic window (longest substring without repeating chars).
- [ ] Substring counts (permutation in string, find all anagrams).
- [ ] Minimum window substring.

#### Binary Search
- [ ] Search space reduction (classic, rotated, 2D matrix).
- [ ] Lower/upper bounds (first/last occurrence).
- [ ] Binary search on answer (allocate books, koko eating bananas).
- [ ] Median of two sorted arrays.

#### Linked Lists
- [ ] Fast and slow pointers (Floyd's cycle detection).
- [ ] In-place reversal (reverse list, reverse in k-groups).
- [ ] Merge sorted lists, reorder list.
- [ ] Copy list with random pointer.

#### Stacks & Queues
- [ ] Next Greater Element using monotonic stacks.
- [ ] LRU Cache.
- [ ] Valid parentheses, min stack.
- [ ] Sliding window maximum (deque).

#### Trees & BSTs
- [ ] Level-order traversals (BFS, zigzag).
- [ ] LCA, height/diameter.
- [ ] Validate BST property.
- [ ] Serialize/deserialize binary tree.
- [ ] Binary tree from inorder + preorder.

#### Heaps & Priority Queues
- [ ] Top-K frequent elements.
- [ ] Merge K sorted lists.
- [ ] Median in data stream.
- [ ] Kth largest element.

#### Graphs
- [ ] BFS, DFS.
- [ ] Kahn's Topological Sort.
- [ ] Dijkstra's algorithm.
- [ ] Disjoint Set Union (DSU).
- [ ] Cycle detection (directed and undirected).
- [ ] Course schedule, network delay time.

#### Dynamic Programming
- [ ] 1D arrays (climbing stairs, house robber, coin change).
- [ ] 2D grid DP (unique paths, min path sum).
- [ ] 0/1 Knapsack variants (subset sum, partition equal).
- [ ] Longest Common Subsequence (LCS).
- [ ] Longest Increasing Subsequence (LIS).
- [ ] Edit distance, word break.

#### Additional Advanced Topics
- [ ] Tries (implement trie, word search II).
- [ ] Bit manipulation (single number, counting bits).
- [ ] Greedy (jump game, activity selection).
- [ ] Backtracking (N-Queens, sudoku solver, permutations).
- [ ] Segment trees / Fenwick trees basics.

---

# PHASE 2 — ADVANCED JAVA + SPRING BOOT + DATABASES (Months 4–6)

---

## 13. Design Principles & GoF Patterns

### Creational Design Patterns `#interview` `#spring`
- [ ] Build thread-safe Singletons (Enum, Bill Pugh, Double-Checked Locking).
- [ ] Implement Factory Method and Abstract Factory to decouple creation from usage.
- [ ] Implement the Builder pattern with validation steps for complex domain entities.

### Structural Design Patterns `#interview` `#spring`
- [ ] Implement Adapter to bridge incompatible legacy interfaces.
- [ ] Implement Decorator dynamically wrapping behaviors (e.g., Java I/O streams).
- [ ] Implement Proxy to handle lazy loading, security checks, and transaction management.
- [ ] Implement Facade to expose unified simplified interfaces over complex sub-systems.

### Behavioral Design Patterns `#interview` `#spring`
- [ ] Implement Strategy to swap runtime algorithms and eliminate branching `if-else` blocks.
- [ ] Implement Observer for decoupled event notification models.
- [ ] Implement Template Method defining invariant algorithm skeletons with abstract hooks.

### Dependency Injection Pattern `#interview` `#spring` `#misc`
- [ ] Compare Constructor Injection vs. Setter Injection vs. Field Injection.
- [ ] Trace why constructor injection is preferred (immutability, mockability, cyclical detection).

### Architecture Patterns (Advanced) `#interview` `#spring` `#misc`
- [ ] Hexagonal / Ports & Adapters architecture.
- [ ] Clean Architecture (Uncle Bob).
- [ ] Onion Architecture.
- [ ] Modular Monolith (very relevant in 2024+).
- [ ] Strangler Fig pattern for legacy migration.
- [ ] Backend for Frontend (BFF).

---

## 14. Database Engineering, Persistence & ORM

### SQL Fundamentals `#interview` `#dsa` `#spring`
- [ ] Write `SELECT`, `INSERT`, `UPDATE`, and `DELETE` statements.
- [ ] Filter and order data using `WHERE`, `ORDER BY`, `LIMIT`, and `OFFSET`.
- [ ] Aggregate using `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, `GROUP BY`, and `HAVING`.
- [ ] Understand `INNER JOIN`, `LEFT JOIN`, and self-joins.
- [ ] Use subqueries and understand when a join is preferable.

### Relational Schema & Data Modeling `#interview` `#spring`
- [ ] Identify entities, attributes, relationships, primary keys, and foreign keys.
- [ ] Design one-to-one, one-to-many, and many-to-many relationships using relational tables.
- [ ] Use junction tables for many-to-many relationships.
- [ ] Understand normalization, denormalization, and referential integrity.
- [ ] Choose indexes based on query/access patterns rather than adding them blindly.

### ACID & Database Transactions `#interview` `#spring`
- [ ] Understand atomicity, consistency, isolation, and durability.
- [ ] Understand commit, rollback, and transaction boundaries.
- [ ] Relate database transactions to application service methods.

### Relational Database Internals & PostgreSQL `#interview` `#spring`
- [ ] Map write paths: Write-Ahead Logging (WAL), page buffers, and dirty page flushing.
- [ ] Trace Multi-Version Concurrency Control (MVCC) isolation and vacuuming mechanics.
- [ ] Compare transaction isolation levels: `READ UNCOMMITTED`, `READ COMMITTED`, `REPEATABLE READ`, `SERIALIZABLE`.
- [ ] Verify isolation anomalies: Dirty Reads, Non-Repeatable Reads, Phantom Reads, Serialization Failures.

### Advanced SQL & Indexing `#interview` `#spring` `#misc`
- [ ] Write complex queries using subqueries, Common Table Expressions (CTEs), and Window Functions (`ROW_NUMBER()`, `RANK()`, `PARTITION BY`).
- [ ] Trace B-Tree index node splits, traversal overhead, and range scan performance.
- [ ] Enforce the Leftmost Prefix Rule on composite index designs.
- [ ] Optimize queries via Covering Indexes to eliminate table heap fetches.
- [ ] Profile query execution plans using `EXPLAIN (ANALYZE, BUFFERS)`.

### JDBC & Connection Pool Tuning `#interview` `#spring`
- [ ] Trace JDBC lifecycle: `DriverManager`, `Connection`, `PreparedStatement`, `ResultSet`.
- [ ] Explain plan caching and SQL injection prevention in `PreparedStatement`.
- [ ] Tune HikariCP: `maximumPoolSize`, `minimumIdle`, `connectionTimeout`, `leakDetectionThreshold`.
- [ ] Apply connection pool sizing formula: `connections = ((core_count × 2) + effective_spindle_count)`.

### Persistence Context & EntityManager `#interview` `#spring`
- [ ] Understand the persistence context as the set of managed entity instances associated with a persistence session.
- [ ] Understand first-level caching, entity identity, dirty checking, flush, and commit.
- [ ] Understand the role of `EntityManager` before using higher-level Spring Data abstractions.

### JPA & Hibernate Entity Lifecycle `#interview` `#spring`
- [ ] Trace entity states: Transient, Managed/Persistent, Detached, Removed.
- [ ] Profile dirty-checking cascades on managed entities during transaction flush.

### JPA Relationship Mappings & Loading `#interview` `#spring`
- [ ] Configure `@OneToOne`, `@OneToMany`, `@ManyToOne`, and `@ManyToMany` mappings.
- [ ] Set bidirectional ownership sides correctly using `mappedBy`.
- [ ] Contrast Lazy loading proxies (`ByteBuddy`) vs. Eager fetch strategies.
- [ ] Diagnose and prevent runtime `LazyInitializationException`.

### Optimistic vs Pessimistic Locking in JPA `#interview` `#spring`
- [ ] Implement optimistic locking with `@Version`.
- [ ] Implement pessimistic locking with `LockModeType.PESSIMISTIC_WRITE`.
- [ ] Handle `OptimisticLockException` and retry strategies.

### The N+1 Query Problem & Remediation `#interview` `#spring` `#misc`
- [ ] Reproduce N+1 query explosions via automated SQL execution logs.
- [ ] Fix N+1 queries using explicit `JOIN FETCH` queries.
- [ ] Fix N+1 queries using Hibernate `@BatchSize(size = 25)`.
- [ ] Fix N+1 queries using dynamic JPA `@EntityGraph` definitions.

### Spring Data JPA `#interview` `#spring`
- [ ] Use `JpaRepository` and understand the repository abstraction.
- [ ] Create derived query methods such as `findByEmail()` and `findByStatusAndCreatedAtAfter()`.
- [ ] Write JPQL with `@Query` and understand when native SQL is appropriate.
- [ ] Use projections, pagination, and sorting.
- [ ] Understand how Spring Data translates repository methods into persistence operations.

### Transaction Management `#interview` `#spring`
- [ ] Configure Spring `@Transactional` boundaries and proxy interceptors.
- [ ] Trace transaction propagation rules: `REQUIRED`, `REQUIRES_NEW`, `NESTED`.
- [ ] Configure rollback rules for checked vs. unchecked exceptions.

### Distributed Caching (Redis) `#interview` `#spring` `#misc`
- [ ] Model data structures: Strings, Hashes, Lists, Sets, and Sorted Sets (ZSET).
- [ ] Implement caching topologies: Cache-Aside, Write-Through, and Write-Behind.
- [ ] Mitigate cache hazards: Cache Stampede (mutex locking), Penetration (Bloom filters), Avalanche (TTL jitter).
- [ ] Configure Redis eviction policies (`allkeys-lru`, `volatile-lru`) and evaluate distributed locking (Redlock).

### Caching Deep-Dive `#interview` `#spring` `#misc`
- [ ] Compare Caffeine (in-JVM) vs Redis (distributed) vs CDN.
- [ ] Implement multi-tier caching (L1 Caffeine + L2 Redis).
- [ ] Understand HTTP caching: ETag, Cache-Control, Vary.
- [ ] Handle cache consistency and near-cache invalidation.

### Database Migrations `#spring` `#misc`
- [ ] Manage schema with Flyway or Liquibase.
- [ ] Version migrations and rollback strategies.
- [ ] Apply migrations in CI/CD pipelines.

### Search Systems `#interview` `#spring` `#misc`
- [ ] Understand Elasticsearch / OpenSearch: indices, mappings, analyzers.
- [ ] Understand inverted index fundamentals.
- [ ] Compare full-text search vs SQL LIKE.
- [ ] Set up Spring Data Elasticsearch.
- [ ] Choose between search vs DB vs cache.

### Data Engineering Touchpoints `#interview` `#misc`
- [ ] Understand CDC (Change Data Capture) with Debezium.
- [ ] Understand ETL vs ELT basics.
- [ ] Understand data lake vs warehouse vs lakehouse (high-level).
- [ ] Batch processing with Spring Batch.
- [ ] Scheduling with Quartz, cron, distributed locks.

### Soft Deletes, Audit Tables, Temporal Data `#spring` `#misc`
- [ ] Implement soft deletes with `deleted_at` timestamp.
- [ ] Implement audit tables with Hibernate Envers.
- [ ] Handle temporal data queries.

### Multi-Tenancy Patterns `#spring` `#misc`
- [ ] Schema-per-tenant vs table-per-tenant vs row-level tenancy.
- [ ] Configure Hibernate multi-tenancy.
- [ ] Handle tenant context propagation.

---

## 15. Spring Boot, Web & Microservices Ecosystem

### Core Spring Framework & IoC Container `#interview` `#spring`
- [ ] Differentiate `BeanFactory` from `ApplicationContext`.
- [ ] Trace Bean Lifecycle: Instantiation → Property Injection → Aware Interfaces → BeanPostProcessor → `@PostConstruct` / InitializingBean → Running → `@PreDestroy`.
- [ ] Manage Bean Scopes: `singleton`, `prototype`, `request`, `session`.
- [ ] Resolve prototype-in-singleton injection mismatch using `ObjectProvider` or scoped proxies.
- [ ] Differentiate component annotations: `@Component`, `@Service`, `@Repository`, `@Controller`, `@RestController`.

### Spring Boot Mechanics & Configuration `#interview` `#spring`
- [ ] Trace auto-configuration loading from `AutoConfiguration.imports`.
- [ ] Apply conditional annotations: `@ConditionalOnProperty`, `@ConditionalOnClass`, `@ConditionalOnMissingBean`.
- [ ] Bind external configurations using type-safe `@ConfigurationProperties`.
- [ ] Monitor health, metrics, and thread dumps using Spring Boot Actuator.

### Configuration, Profiles & Environment Management `#interview` `#spring`
- [ ] Configure applications using `application.properties` and `application.yml`.
- [ ] Use Spring profiles for environment-specific configuration.
- [ ] Bind configuration using `@ConfigurationProperties`.
- [ ] Understand environment variables, configuration precedence, and why secrets should not be committed to source control.

### API Error Design & Validation Responses `#interview` `#spring`
- [ ] Design consistent error responses for validation, authentication, authorization, not-found, conflict, and unexpected failures.
- [ ] Understand RFC 7807 Problem Details and structured API error payloads.
- [ ] Separate internal exception details from information safe to expose to clients.

### REST API Architecture & Web MVC `#interview` `#spring`
- [ ] Trace request dispatching through `DispatcherServlet`, `HandlerMapping`, and `HandlerAdapter`.
- [ ] Validate HTTP semantics: GET, POST, PUT, PATCH, DELETE idempotency and safety rules.
- [ ] Return appropriate HTTP response statuses: 200, 201, 204, 400, 401, 403, 404, 409, 500.
- [ ] Enforce payload constraints using Jakarta Bean Validation (`@Valid`, `@NotNull`, `@Size`).
- [ ] Catch exceptions centrally using `@ControllerAdvice` and `@ExceptionHandler` emitting RFC 7807 payloads.
- [ ] Configure JSON serialization with Jackson: handle cycles via `@JsonIgnore` / `@JsonBackReference`.

### Spring Security & Authentication Architecture `#interview` `#spring`
- [ ] Trace request handling through the `SecurityFilterChain`.
- [ ] Extract credentials into `Authentication` tokens managed by `SecurityContextHolder`.
- [ ] Implement stateless JWT authentication: cryptographic signing, expiration, and refresh rotations.
- [ ] Enforce Method-Level Authorization using `@PreAuthorize("hasRole('ADMIN')")`.
- [ ] Configure CORS filters and evaluate CSRF protections in stateless APIs.

### OAuth 2.0, OIDC & Enterprise SSO `#interview` `#spring` `#misc`
- [ ] Understand OAuth 2.0 flows: Authorization Code + PKCE, Client Credentials, Refresh Token.
- [ ] Understand OpenID Connect (OIDC) as identity layer on OAuth 2.0.
- [ ] Understand SAML basics for enterprise SSO.
- [ ] Integrate Keycloak / Auth0 / Okta with Spring Security.
- [ ] Understand password hashing: bcrypt, Argon2, scrypt — why not SHA-256.
- [ ] Use secrets management: HashiCorp Vault, AWS Secrets Manager.
- [ ] Mitigate OWASP Top 10 in Spring applications.
- [ ] Configure security headers: CSP, HSTS, X-Frame-Options.
- [ ] Understand SQL injection, XSS, CSRF, SSRF, XXE.
- [ ] Apply certificate pinning and mTLS for service-to-service.

### Webhooks & Outbound Integrations `#spring` `#misc`
- [ ] Design outbound webhook delivery with retries and exponential backoff.
- [ ] Sign webhooks with HMAC for authenticity.
- [ ] Handle idempotency on receiver side.
- [ ] Implement dead-letter queues for failed deliveries.

### Payments & Third-Party Integrations `#spring` `#misc`
- [ ] Integrate Stripe / Razorpay payment flows.
- [ ] Handle webhook idempotency for payment events.
- [ ] Understand PCI-DSS basics at a high level.

### Email, SMS & Notifications `#spring` `#misc`
- [ ] Send email with JavaMail / SendGrid.
- [ ] Send SMS with Twilio.
- [ ] Integrate push notifications (FCM, APNs).

### PDF / Excel Generation `#spring` `#misc`
- [ ] Generate PDFs with iText / OpenPDF.
- [ ] Generate Excel with Apache POI.
- [ ] Stream large exports without OOM.

### Microservices Design Patterns `#interview` `#spring` `#misc`
- [ ] Define microservice boundaries using Domain-Driven Design (DDD) bounded contexts.
- [ ] Implement API Gateway patterns: reverse routing, SSL termination, and rate-limiting.
- [ ] Configure Client-Side and Server-Side Service Discovery models.
- [ ] Implement Resilience Patterns using Resilience4j: Circuit Breaker, Retry with Backoff, Bulkhead Isolation.
- [ ] Coordinate distributed transactions using the Saga Pattern (Choreography vs. Orchestration).
- [ ] Prevent data loss in event-driven systems using the Transactional Outbox Pattern.

### Spring Cloud Ecosystem `#interview` `#spring` `#misc`
- [ ] Set up Spring Cloud Gateway (routing, filters, predicates).
- [ ] Set up Spring Cloud Config Server.
- [ ] Configure Eureka / Consul for service discovery.
- [ ] Use Spring Cloud Circuit Breaker abstraction.
- [ ] Use Spring Cloud Stream for message-driven microservices.
- [ ] Migrate Spring Cloud Sleuth → Micrometer Tracing.

### Domain-Driven Design (DDD) `#interview` `#spring` `#misc`
- [ ] Understand aggregates, entities, value objects.
- [ ] Understand repositories (DDD sense, not Spring Data).
- [ ] Model domain events.
- [ ] Apply Anti-Corruption Layer.
- [ ] Distinguish strategic vs tactical DDD.
- [ ] Run Event Storming sessions.

### Event Sourcing & CQRS `#interview` `#misc`
- [ ] Design an event store.
- [ ] Implement projections and read models.
- [ ] Apply snapshotting.
- [ ] Handle replay and rebuild.
- [ ] Understand when NOT to use event sourcing.

### Reactive Programming (WebFlux) `#interview` `#spring` `#misc`
- [ ] Learn Project Reactor: `Mono`, `Flux`, operators, backpressure.
- [ ] Compare Spring WebFlux vs Spring MVC.
- [ ] Use reactive repositories: R2DBC, reactive Redis, reactive Kafka.
- [ ] Understand when reactive helps (I/O-bound, streaming) and when it hurts (complexity).
- [ ] Understand virtual threads vs reactive — the 2024+ debate.

### Spring AI Integration `#interview` `#spring` `#misc`
- [ ] Connect Spring applications to LLM endpoints using `ChatClient` and `ChatModel`.
- [ ] Generate typed JSON payloads by parsing model outputs directly into Java records/POJOs.
- [ ] Build contextual prompt pipelines supporting Retrieval-Augmented Generation (RAG).
- [ ] Understand vector databases: Pinecone, Weaviate, pgvector.
- [ ] Understand embeddings fundamentals.
- [ ] Understand function calling / tool use.
- [ ] Mitigate prompt injection risks.
- [ ] Reason about LLM cost/latency trade-offs.

---

# PHASE 3 — DISTRIBUTED SYSTEMS + CLOUD + KAFKA (Months 7–9)

---

## 16. Kafka & Message Brokers `#interview` `#spring` `#misc`

### Kafka Core Architecture
- [ ] Understand brokers, topics, partitions, offsets, consumer groups.
- [ ] Trace partition assignment and rebalancing.
- [ ] Understand replication factor and ISR (In-Sync Replicas).
- [ ] Understand log segments and retention policies.
- [ ] Understand `__consumer_offsets` topic.

### Producer Semantics
- [ ] Configure acks: 0, 1, all.
- [ ] Enable idempotent producer.
- [ ] Use producer transactions for exactly-once.
- [ ] Tune batching, compression, and linger.

### Consumer Semantics
- [ ] Compare auto-commit vs manual commit.
- [ ] Handle rebalancing with `ConsumerRebalanceListener`.
- [ ] Tune `max.poll.records` and `max.poll.interval.ms`.
- [ ] Understand consumer lag monitoring.

### Delivery Guarantees
- [ ] At-most-once: fire and forget.
- [ ] At-least-once: retries + idempotent consumer.
- [ ] Exactly-once: idempotent producer + transactional consumer.

### Advanced Kafka
- [ ] Understand log compaction.
- [ ] Understand retention policies (time, size, compact).
- [ ] Learn Kafka Streams / ksqlDB basics.
- [ ] Implement Dead Letter Queues (DLQ) and retry topics.
- [ ] Handle poison pill messages.
- [ ] Use Schema Registry with Avro/Protobuf evolution.

### Spring Kafka Integration
- [ ] Configure `@KafkaListener` and `KafkaTemplate`.
- [ ] Configure error handlers and retry.
- [ ] Configure `@RetryableTopic` for non-blocking retries.
- [ ] Configure batch listeners.

### Broker Comparison
- [ ] Compare RabbitMQ vs Kafka vs SQS vs Pulsar.
- [ ] Choose the right broker for a use case.

---

## 17. Observability & Monitoring `#interview` `#spring` `#misc`

### Structured Logging
- [ ] Configure JSON logging with SLF4J + Logback / Log4j2.
- [ ] Use MDC for correlation IDs across threads (ties into ThreadLocal topic).
- [ ] Set up centralized logging: ELK Stack (Elasticsearch, Logstash, Kibana) or Loki + Grafana.
- [ ] Configure log levels and dynamic log level changes.

### Metrics
- [ ] Instrument with Micrometer → Prometheus → Grafana.
- [ ] Apply RED method (Rate, Errors, Duration).
- [ ] Apply USE method (Utilization, Saturation, Errors).
- [ ] Configure alerts: SLOs, SLIs, error budgets.

### Distributed Tracing
- [ ] Set up OpenTelemetry instrumentation.
- [ ] Trace requests with Jaeger / Zipkin.
- [ ] Propagate trace/span context across services.
- [ ] Understand sampling strategies.

### Health Checks & Probes
- [ ] Configure Actuator health endpoints.
- [ ] Understand liveness vs readiness probes (Kubernetes context).
- [ ] Configure startup probes.

---

## 18. DevOps, Infrastructure & System Delivery

### Build Automation Tools (Maven & Gradle) `#interview` `#misc`
- [ ] Step through Maven lifecycle phases: `validate`, `compile`, `test`, `package`, `verify`, `install`, `deploy`.
- [ ] Manage dependency scopes: `compile`, `provided`, `runtime`, `test`.
- [ ] Resolve transitive dependency version conflicts via nearest-wins rules and `<exclusions>`.
- [ ] Optimize incremental builds and task caching using Gradle (`implementation` vs. `api`).

### Version Control Systems (Git) `#interview` `#misc`
- [ ] Compare branching strategies: Git Flow vs. Trunk-Based Development.
- [ ] Contrast history preservation: `git merge` (3-way / fast-forward) vs. linear `git rebase`.
- [ ] Clean up commit histories interactively using `git rebase -i`.
- [ ] Apply individual commits across branches using `git cherry-pick`.
- [ ] Resolve merge conflict markers and manage state with `git stash`.
- [ ] Contrast state modifications: `git reset` (soft, mixed, hard) vs. safe `git revert`.

### CI/CD Pipelines `#interview` `#misc`
- [ ] Author GitHub Actions / GitLab CI pipelines.
- [ ] Use artifact repositories: Nexus, Artifactory.
- [ ] Apply semantic versioning and release trains.
- [ ] Use feature flags: LaunchDarkly, Unleash, Togglz.
- [ ] Implement blue-green vs canary vs rolling deployments.
- [ ] Run database migrations in CI (Flyway, Liquibase).
- [ ] Generate SBOM and address supply chain security (SLSA, Sigstore).

### Docker Networking & Service Communication `#interview` `#spring` `#misc`
- [ ] Understand bridge networks, container DNS, published ports, and service-to-service communication.
- [ ] Connect a Spring Boot container to PostgreSQL/Redis containers using Compose service names rather than `localhost`.
- [ ] Configure environment variables and container healthchecks for dependent services.

### Containerization (Docker) `#interview` `#misc`
- [ ] Trace Linux isolation primitives: namespaces, cgroups, and layered filesystems.
- [ ] Write optimized multi-stage Dockerfiles for lean, minimal Java runtime images.
- [ ] Enforce container security by running processes under non-root users.
- [ ] Configure network modes (Bridge, Host) and manage persistent data with Named Volumes.
- [ ] Orchestrate multi-container local environments using Docker Compose.

### Kubernetes `#interview` `#misc`
- [ ] Understand Pods, ReplicaSets, Deployments, StatefulSets.
- [ ] Configure Services (ClusterIP, NodePort, LoadBalancer), Ingress.
- [ ] Use ConfigMaps and Secrets, environment injection.
- [ ] Configure resource requests/limits, HPA (Horizontal Pod Autoscaler).
- [ ] Configure liveness, readiness, startup probes.
- [ ] Implement rolling updates, blue-green, canary deployments.
- [ ] Write Helm charts basics.
- [ ] Understand Namespaces, RBAC, Network Policies.
- [ ] Master `kubectl` essentials: `get`, `describe`, `logs`, `exec`, `port-forward`.

### Cloud Architecture (AWS) `#interview` `#misc`
- [ ] Provision compute instances via EC2 and containerized workloads via ECS with Fargate.
- [ ] Provision managed persistence via RDS PostgreSQL (multi-AZ replication, automated snapshots).
- [ ] Store application assets securely in S3 using pre-signed URLs and lifecycle policies.
- [ ] Construct cloud networks: VPC, Public/Private subnets, Route Tables, Internet Gateways, NAT Gateways.
- [ ] Configure firewall rules with Security Groups vs. Network ACLs (NACLs).
- [ ] Enforce Principle of Least Privilege using IAM roles and fine-grained policies.

### Serverless (AWS Lambda) `#interview` `#misc`
- [ ] Build AWS Lambda + API Gateway + DynamoDB.
- [ ] Understand cold starts and mitigation.
- [ ] Use Spring Cloud Function.
- [ ] Choose when serverless beats containers.

### Linux & Shell Fundamentals `#misc`
- [ ] Master `top`, `htop`, `netstat`, `ss`, `lsof`, `strace`, `tcpdump`.
- [ ] Write shell scripts for ops tasks.
- [ ] Understand file permissions, users, groups.

### Agile Engineering & Delivery Workflows `#interview` `#misc`
- [ ] Contrast delivery models: Scrum (fixed sprints, velocity) vs. Kanban (continuous flow, WIP limits).
- [ ] Participate in engineering ceremonies: Sprint Planning, Daily Standups, Reviews, Retrospectives.
- [ ] Estimate engineering tasks using Story Points, Planning Poker, and manage Jira workflows.

---

## 19. Testing & Software Quality

### Unit Testing with JUnit 5 `#interview` `#spring`
- [ ] Manage test lifecycle phases: `@BeforeAll`, `@BeforeEach`, `@AfterEach`, `@AfterAll`.
- [ ] Write fluent, expressive state assertions using AssertJ.
- [ ] Organize complex test suites hierarchically using `@Nested` test classes.
- [ ] Execute data-driven test scenarios using `@ParameterizedTest` (`@ValueSource`, `@CsvSource`).

### Mocking Frameworks (Mockito) `#interview` `#spring`
- [ ] Isolate dependencies using `@Mock`, `@InjectMocks`, and partial mock wrappers (`@Spy`).
- [ ] Stub behavior using `when().thenReturn()` and `doThrow()`.
- [ ] Verify execution counts (`verify(mock, times(1))`) and capture arguments with `ArgumentCaptor`.
- [ ] Differentiate test doubles: Dummies, Stubs, Mocks, Spies, and Fakes.

### Spring Integration Testing `#interview` `#spring`
- [ ] Run full-context integration tests using `@SpringBootTest`.
- [ ] Isolate web-layer controllers using sliced `@WebMvcTest` configurations.
- [ ] Isolate database repositories using sliced `@DataJpaTest` configurations.
- [ ] Spin up real PostgreSQL and Redis test instances dynamically using Testcontainers.

### Advanced Testing `#interview` `#spring` `#misc`
- [ ] Stub HTTP with WireMock / MockWebServer.
- [ ] Assert async with Awaitility.
- [ ] Run mutation testing with PIT.
- [ ] Property-based testing with jqwik.
- [ ] Chaos engineering basics (Chaos Monkey, Litmus).
- [ ] Run load tests in CI.
- [ ] Build test data builders / object mothers.

### Testing Pyramid & Test Strategy `#interview` `#spring`
- [ ] Distinguish unit, integration, contract, and end-to-end tests.
- [ ] Understand why most tests should remain fast and isolated while a smaller number exercise real infrastructure.
- [ ] Choose between mocks, test slices, full-context tests, and Testcontainers based on the behavior being verified.

### Methodologies & Quality Metrics `#interview` `#misc`
- [ ] Apply the Test-Driven Development (TDD) cycle: Red → Green → Refactor.
- [ ] Profile branch and line test coverage targets using JaCoCo.

---

## 20. Performance Engineering & Profiling `#interview` `#misc`

- [ ] Use JFR (Java Flight Recorder) and JMC.
- [ ] Profile CPU and allocation flamegraphs with Async-profiler.
- [ ] Write JMH microbenchmarks.
- [ ] Run load tests: JMeter, Gatling, k6.
- [ ] Tune latency vs throughput.
- [ ] Reason about p50, p95, p99, p999 latency.
- [ ] Diagnose tail latency causes: GC, queuing, retries, head-of-line blocking.

---

# PHASE 4 — SYSTEM DESIGN + INTERVIEW PREP (Months 10–12)

---

## 21. Modern Frontend Integration (AI-Assisted)

### React & Next.js Core Concepts `#misc`
- [ ] Manage immutable component state and side-effects via hooks (`useState`, `useEffect`).
- [ ] Optimize performance using `useCallback`, `useMemo`, and persistent `useRef`.
- [ ] Architecture design: React Server Components (RSC) vs. Client Components (`'use client'`).
- [ ] Manage client data caches and optimistic updates using TanStack Query (React Query) / SWR.

### Component Systems & UI Styling `#misc`
- [ ] Build responsive, utility-first interfaces using Tailwind CSS.
- [ ] Assemble accessible UI design systems using `shadcn/ui` (Radix primitives).
- [ ] Enforce route security on protected pages using Next.js Edge Middleware.
- [ ] Store authenticated JWT tokens securely inside HTTP-only cookies.

### AI-Assisted Frontend Development `#misc`
- [ ] Use v0.dev / Cursor / Claude to scaffold UI rapidly.
- [ ] Generate components from Figma designs with AI tools.
- [ ] Use AI for test generation and refactoring.
- [ ] Review AI output critically — never ship unreviewed.
- [ ] Use AI for accessibility audits and responsive fixes.

---

## 22. System Design & Distributed Architecture (HLD) `#interview` `#misc`

### Back-of-the-Envelope Estimation `#interview` `#misc`
- [ ] Estimate QPS from DAU (daily active users).
- [ ] Estimate storage: rows × row size × retention.
- [ ] Estimate bandwidth: QPS × payload size.
- [ ] Estimate memory for caches (working set size).
- [ ] Memorize latency numbers every engineer should know (L1, L2, RAM, SSD, network, disk seek).
- [ ] Use power-of-two approximations: 1 day ≈ 86,400 sec ≈ 10^5 sec.

### Distributed Foundations & Tradeoffs `#interview` `#misc`
- [ ] Analyze trade-offs in distributed systems using the CAP Theorem and PACELC Theorem.
- [ ] Plan scaling paths: Vertical compute upgrades vs. Horizontal stateless scaling.

### Consistency & Distributed Transactions `#interview` `#misc`
- [ ] Implement idempotency keys for REST APIs.
- [ ] Understand Two-Phase Commit (2PC) and why it's avoided.
- [ ] Apply TCC (Try-Confirm-Cancel) pattern.
- [ ] Understand eventual consistency and read-your-writes.
- [ ] Understand vector clocks / Lamport timestamps basics.
- [ ] Understand distributed locks: Redlock critique, ZooKeeper, etcd.

### Traffic Management & Load Balancing `#interview` `#misc`
- [ ] Contrast Layer 4 (Transport/TCP) vs. Layer 7 (Application/HTTP) load balancers.
- [ ] Trace routing algorithms: Round Robin, Weighted Least Connections, Consistent Hashing.

### Asynchronous Decoupling & Event Brokers `#interview` `#misc`
- [ ] Contrast Point-to-Point message queues vs. Distributed Publish-Subscribe logs.
- [ ] Differentiate delivery semantics: At-Most-Once, At-Least-Once, Exactly-Once.
- [ ] Implement idempotent consumers to guarantee deduplicated message processing.

### Rate Limiting (Distributed) `#interview` `#misc`
- [ ] Implement distributed rate limiting with Redis + Lua scripts.
- [ ] Use consistent hashing for rate limiter sharding.
- [ ] Configure API Gateway-level rate limiting.

### Database Partitioning & High Availability `#interview` `#misc`
- [ ] Configure Read Replicas and address replication lag in read-heavy applications.
- [ ] Partition large data sets horizontally via database sharding strategies (Hash vs. Range).

### Classic System Design Problems `#interview` `#misc`
- [ ] Design URL shortener (TinyURL).
- [ ] Design Twitter / Instagram feed.
- [ ] Design WhatsApp / chat system.
- [ ] Design Uber / ride-sharing.
- [ ] Design YouTube / Netflix streaming.
- [ ] Design Dropbox / Google Drive.
- [ ] Design Rate Limiter (distributed).
- [ ] Design Notification System.
- [ ] Design Search Autocomplete.
- [ ] Design Web Crawler.

---

## 23. Behavioral & Leadership Interview Prep `#interview`

- [ ] Master the STAR method for behavioral questions.
- [ ] Prepare 8–10 conflict resolution stories.
- [ ] Prepare technical leadership stories: mentoring, code review culture.
- [ ] Prepare incident response and postmortem stories.
- [ ] Practice trade-off articulation (the #1 senior signal).
- [ ] Prepare "Tell me about a time you disagreed with a decision."
- [ ] Prepare "Tell me about your most complex project."
- [ ] Prepare "Why this company?" for each target.
- [ ] Prepare salary negotiation basics.

---

## 24. Interview Meta-Skills `#interview`

- [ ] Coding interview: think aloud, clarify, edge cases, complexity.
- [ ] System design interview flow: requirements → estimation → API → data model → scaling → trade-offs.
- [ ] Take-home review checklist.
- [ ] Prepare 5–10 thoughtful "Do you have questions for us?" questions.
- [ ] Practice with mock interviews (Pramp, interviewing.io, peers).
- [ ] Record yourself and review body language.
- [ ] Build a portfolio README for your flagship project.

---

# PHASE 5 — FLAGSHIP PRODUCTION PROJECT (Runs Parallel, Months 4–12)

---

## Enterprise Full-Stack Application (Spring Boot + Next.js + PostgreSQL + Redis + Kafka + Docker + AWS)

### Phase 1: Security & Identity Foundation
- [ ] Implement stateless authentication with asymmetric/symmetric JWT in HTTP-only cookies.
- [ ] Implement refresh token rotation and database revocation tables.
- [ ] Configure multi-role authorization checks (Admin, User, Auditor).
- [ ] Integrate OAuth 2.0 (Google/GitHub login).
- [ ] Implement rate limiting on auth endpoints.
- [ ] Add password reset and email verification flows.

### Phase 2: Data Modeling & Persistence Engine
- [ ] Build complex relational schemas with bidirectional JPA entities.
- [ ] Optimize read queries using explicit `@EntityGraph` mappings to eliminate N+1 queries.
- [ ] Configure HikariCP connection pooling and verify zero connection leaks.
- [ ] Implement optimistic locking with `@Version`.
- [ ] Add Flyway/Liquibase migrations.
- [ ] Implement soft deletes and audit tables (Hibernate Envers).

### Phase 3: High-Performance Caching Layer
- [ ] Implement Cache-Aside workflows for hot read paths using Redis.
- [ ] Apply TTL jitter strategies to prevent cache avalanche failures.
- [ ] Add multi-tier caching (Caffeine L1 + Redis L2).
- [ ] Implement cache invalidation on write paths.

### Phase 4: Asynchronous Processing & Micro-Integrations
- [ ] Offload heavy background workloads via Spring `@Async` and custom `ThreadPoolExecutor` configurations.
- [ ] Integrate real-time notification pushes (WebSockets or Server-Sent Events).
- [ ] Integrate Spring AI endpoints for automated entity processing or summary generation.
- [ ] Integrate Kafka for event-driven workflows.
- [ ] Implement Transactional Outbox pattern.
- [ ] Implement Saga pattern for a cross-service workflow.

### Phase 5: Observability
- [ ] Add structured JSON logging with correlation IDs.
- [ ] Add Micrometer metrics → Prometheus → Grafana.
- [ ] Add OpenTelemetry tracing → Jaeger.
- [ ] Add Actuator health checks.
- [ ] Configure alerting rules.

### Phase 6: Automated Testing & Verification
- [ ] Achieve robust line/branch coverage across domain services using JUnit 5 and Mockito.
- [ ] Implement integration test suites backed by disposable Testcontainers instances.
- [ ] Add contract tests with Pact or WireMock.
- [ ] Add load tests with Gatling or k6.
- [ ] Add mutation testing with PIT.

### Phase 7: Containerization & Cloud Deployment
- [ ] Write multi-stage Dockerfiles building lightweight production images.
- [ ] Construct a local development topology using Docker Compose.
- [ ] Provision AWS infrastructure: VPC subnets, RDS PostgreSQL, and ECS Fargate compute.
- [ ] Deploy the Next.js frontend and establish public-facing routing via an Application Load Balancer.
- [ ] Configure CI/CD pipeline with GitHub Actions.
- [ ] Add Kubernetes manifests (or Helm chart) for deployment.
- [ ] Configure secrets with AWS Secrets Manager / Vault.

### Phase 8: Documentation & Portfolio Polish
- [ ] Write a comprehensive README with architecture diagram.
- [ ] Record a 5-minute demo video.
- [ ] Write a technical blog post about one hard problem solved.
- [ ] Add API documentation with OpenAPI/Swagger.
- [ ] Create a portfolio site linking to the project.

---

## 25. Java Language Evolution (Java 21–23) `#interview` `#misc`

- [ ] Pattern matching for switch (finalized Java 21).
- [ ] Record patterns (deconstruction).
- [ ] Sequenced collections (`SequencedCollection`, `SequencedMap`).
- [ ] Structured Concurrency (preview).
- [ ] Scoped Values (preview, replaces ThreadLocal in many cases).
- [ ] String templates (preview).
- [ ] Unnamed classes and instance main methods.
- [ ] Generational ZGC (Java 21).
- [ ] Foreign Function & Memory API (Panama).
- [ ] Understand migration paths: Java 8 → 11 → 17 → 21.
- [ ] Migrate Spring Boot 2 → 3 (Jakarta namespace).

---

## 26. Legacy & Migration Skills `#interview` `#misc`

- [ ] Migrate Java 8 → 17/21 in a sample project.
- [ ] Migrate Spring Boot 2 → 3 (Jakarta namespace changes).
- [ ] Understand Monolith → microservices strangler pattern.
- [ ] Understand Struts/JSF → Spring migration basics.

---

# 📊 PROGRESS TRACKING

| Section                                             | Total Tasks | Completed | Progress |
| --------------------------------------------------- | ----------- | --------- | -------- |
| 1. Language Fundamentals & Execution Model          | 82          | 0         | 0%       |
| 2. OOP & Low-Level Design (LLD)                     | 55          | 0         | 0%       |
| 3. JVM Architecture & Memory Management             | 22          | 0         | 0%       |
| 4. Exception Handling                               | 14          | 0         | 0%       |
| 5. Generics & Type Erasure                          | 14          | 0         | 0%       |
| 6. Collections Framework Internals                  | 32          | 0         | 0%       |
| 7. Functional Programming & Streams API             | 18          | 0         | 0%       |
| 8. Multithreading & High Concurrency                | 48          | 0         | 0%       |
| 9. I/O, Serialization & Networking Basics           | 12          | 0         | 0%       |
| 10. HTTP, REST & Web Fundamentals                   | 40          | 0         | 0%       |
| 11. Reflection, Annotations & Framework Foundations | 7           | 0         | 0%       |
| 12. DSA Track                                       | 50          | 0         | 0%       |
| 13. Design Principles & GoF Patterns                | 16          | 0         | 0%       |
| 14. Database Engineering, Persistence & ORM         | 60          | 0         | 0%       |
| 15. Spring Boot, Web & Microservices Ecosystem      | 65          | 0         | 0%       |
| 16. Kafka & Message Brokers                         | 30          | 0         | 0%       |
| 17. Observability & Monitoring                      | 15          | 0         | 0%       |
| 18. DevOps, Infrastructure & System Delivery        | 60          | 0         | 0%       |
| 19. Testing & Software Quality                      | 30          | 0         | 0%       |
| 20. Performance Engineering & Profiling             | 7           | 0         | 0%       |
| 21. Modern Frontend Integration                     | 9           | 0         | 0%       |
| 22. System Design & Distributed Architecture (HLD)  | 35          | 0         | 0%       |
| 23. Behavioral & Leadership Interview Prep          | 9           | 0         | 0%       |
| 24. Interview Meta-Skills                           | 7           | 0         | 0%       |
| 25. Flagship Production Project                     | 40          | 0         | 0%       |
| 26. Java Language Evolution (21–23)                 | 11          | 0         | 0%       |
| 27. Legacy & Migration Skills                       | 4           | 0         | 0%       |

---
