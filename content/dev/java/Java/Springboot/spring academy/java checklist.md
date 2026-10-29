
# PART 1

## 1. Language Fundamentals & Execution Model

- [ ] **Primitive Types vs Wrapper Classes** `#interview`
    
      
    - [ ] Map stack vs. heap storage for `int` vs. `Integer`
        
          
        
    - [ ] Trace memory overhead of object headers on wrappers
        
          
        
    - [ ] Profile autoboxing/unboxing performance costs in hot loops
        
          
        
- [ ] **Autoboxing / Unboxing & The Integer Cache Pool** `#interview` `#misc`
    
      
    - [ ] Verify `-128` to `127` pool caching behavior
        
          
        
    - [ ] Debug equality traps (`Integer a = 127; Integer b = 127; a == b` vs `a = 128`)
        
          
        
    - [ ] Inspect JVM flag `-XX:AutoBoxCacheMax`
        
          
        
- [ ] **Pass-By-Value Mechanics** `#interview`
    
      
    - [ ] Trace object reference passing by value in stack frames
        
          
        
    - [ ] Demonstrate object state mutation vs. reassignment isolation
        
          
        
- [ ] **Type Casting & Conversions** `#interview`
    
      
    - [ ] Trace narrowing truncation (`float` to `int`) and bit wrapping (`long` to `int`)
        
          
        
    - [ ] Verify primitive promotion rules in arithmetic expressions
        
          
        
- [ ] **String Pool Mechanics & Allocation** `#interview` `#misc`
    
      
    - [ ] Diagram String pool in Heap (post-Java 7) vs. Metaspace/PermGen
        
          
        
    - [ ] Compare string literals vs. explicit `new String()` allocations
        
          
        
    - [ ] Implement and verify `String.intern()` memory deduplication
        
          
        
- [ ] **String Immutability** `#interview` `#misc`
    
      
    - [ ] Explain security implications (class loading, network sockets, DB credentials)
        
          
        
    - [ ] Analyze `hashCode()` caching mechanics and thread-safety invariants
        
          
        
    - [ ] Trace why `String` is marked `final` in bytecode
        
          
        
- [ ] **String Performance & Concatenation** `#interview` `#misc`
    
      
    - [ ] Profile $O(n^2)$ loop penalty with `+` operator
        
          
        
    - [ ] Compare `StringBuilder` vs. `StringBuffer` (synchronization overhead)
        
          
        
    - [ ] Trace default capacity growth factor ($2n + 2$)
        
          
        
- [ ] **Structural vs Reference Equality** `#interview`
    
      
    - [ ] Implement `equals()` satisfying reflexive, symmetric, transitive, consistent contracts
        
          
        
    - [ ] Audit null-safety invariants (`"constant".equals(variable)`)
        
          
        
- [ ] **The HashCode Contract** `#interview` `#dsa`
    
      
    - [ ] Enforce consistency between `equals()` and `hashCode()`
        
          
        
    - [ ] Trace hash bucket distribution and collisions
        
          
        
    - [ ] Reproduce map lookup failure caused by mutating keyed object fields
        
          
        
- [ ] **Core Language Keywords** `#interview`
    
      
    - [ ] Distinguish `final` reference reassignment vs. internal object mutation
        
          
        
    - [ ] Sequence static initialization blocks vs. instance blocks
        
          
        
    - [ ] Trace `transient` behavior in serialization
        
          
        
    - [ ] Analyze `volatile` visibility barriers and `strictfp` precision limits
        
          
        
- [ ] **Variable Argument Methods (Varargs)** `#interview` `#misc`
    
      
    - [ ] Trace compiler transformation of `T...` into array instantiations
        
          
        
    - [ ] Resolve method overloading ambiguity edge cases
        
          
        
- [ ] **Bitwise & Shift Operators** `#interview` `#dsa` `#misc`
    
      
    - [ ] Implement in-place XOR bit swaps and bitmask extractions
        
          
        
    - [ ] Contrast signed right shift (`>>`) vs. unsigned logical shift (`>>>`)
        
          
        
    - [ ] Apply bitwise AND masking (`hash & (n - 1)`) as used in `HashMap`
        
          
        

## 2. Object-Oriented Programming & Low-Level Design (LLD)

- [ ] **Core OOP Pillars in Production Code** `#interview`
    
      
    - [ ] Implement Encapsulation via defensive copying and immutable access boundaries
        
          
        
    - [ ] Implement Polymorphism via unified domain interfaces (e.g., payment gateways)
        
          
        
    - [ ] Implement Abstraction separating interface contracts from execution logic
        
          
        
- [ ] **Method Overloading vs Overriding** `#interview`
    
      
    - [ ] Trace compile-time static binding vs. runtime dynamic dispatch
        
          
        
    - [ ] Demonstrate static method shadowing/hiding vs. overriding
        
          
        
- [ ] **Runtime Polymorphism Internals** `#interview` `#misc`
    
      
    - [ ] Diagram Virtual Method Table (vtable) lookups in the JVM
        
          
        
    - [ ] Explain why `private`, `static`, and `final` methods bypass vtables
        
          
        
- [ ] **Covariant Return Types** `#interview` `#misc`
    
      
    - [ ] Implement method overriding returning narrowed subclass types
        
          
        
    - [ ] Apply covariant return types in fluent builder implementations
        
          
        
- [ ] **Composition vs Inheritance** `#interview` `#spring`
    
      
    - [ ] Identify fragile base class anti-patterns
        
          
        
    - [ ] Refactor inheritance hierarchies into composition via the Delegation Pattern
        
          
        
- [ ] **Instanceof Pattern Matching (Java 16+)** `#interview`
    
      
    - [ ] Replace explicit casting blocks with `if (obj instanceof String s)`
        
          
        
    - [ ] Apply pattern matching guards inside switch expressions
        
          
        
- [ ] **Object Base Methods** `#interview`
    
      
    - [ ] Override `toString()`, `clone()`, and `getClass()`
        
          
        
    - [ ] Evaluate why `finalize()` was deprecated and removed
        
          
        
    - [ ] Trace inter-thread signaling with `wait()`, `notify()`, and `notifyAll()`
        
          
        
- [ ] **Object Instantiation Sequence** `#interview`
    
      
    - [ ] Step through execution: Parent static $\to$ Child static $\to$ Parent instance $\to$ Parent constructor $\to$ Child instance $\to$ Child constructor
        
          
        
- [ ] **Immutable Class Design Recipe** `#interview` `#spring` `#misc`
    
      
    - [ ] Mark class `final` and all fields `private final`
        
          
        
    - [ ] Perform defensive copies in constructors for mutable fields (e.g., `Date`, `List`)
        
          
        
    - [ ] Return unmodifiable views or cloned instances from getter methods
        
          
        
- [ ] **Abstract Classes vs Interfaces** `#interview` `#spring`
    
      
    - [ ] Evaluate tradeoffs: shared mutable state vs. multiple contract inheritance
        
          
        
    - [ ] Resolve Diamond Problem conflicts in multiple interface default methods
        
          
        
- [ ] **Functional Interfaces** `#interview` `#spring`
    
      
    - [ ] Validate Single Abstract Method (SAM) rule with `@FunctionalInterface`
        
          
        
    - [ ] Contrast lambda implementation mechanics against anonymous inner classes
        
          
        
- [ ] **Marker Interfaces** `#interview`
    
      
    - [ ] Inspect JVM runtime type-checking for `Serializable` and `Cloneable`
        
          
        
    - [ ] Create custom marker interfaces for domain processing pipelines
        
          
        
- [ ] **Java Records** `#interview` `#misc`
    
      
    - [ ] Construct data-carrier records using `record Point(int x, int y) {}`
        
          
        
    - [ ] Customize canonical and compact record constructors
        
          
        
    - [ ] Inspect generated bytecode for immutability and final fields
        
          
        
- [ ] **Sealed Classes & Interfaces** `#interview` `#misc`
    
      
    - [ ] Define restricted class trees using `sealed`, `permits`, `non-sealed`, and `final`
        
          
        
    - [ ] Implement exhaustive compiler-checked switch expressions without default branches
        
          
        
- [ ] **Enum Deep-Dive** `#interview` `#misc`
    
      
    - [ ] Implement constant-specific abstract methods within enums
        
          
        
    - [ ] Benchmark `EnumMap` and `EnumSet` bit-vector performance
        
          
        
    - [ ] Construct thread-safe, serialization-safe singletons via enums
        
          
        
- [ ] **SOLID Principles Implementation** `#interview` `#spring`
    
      
    - [ ] Single Responsibility: Decouple business domains from validation and persistence
        
          
        
    - [ ] Open/Closed: Extend processing logic via Strategy pattern interfaces
        
          
        
    - [ ] Liskov Substitution: Verify derived classes never throw unexpected contract exceptions
        
          
        
    - [ ] Interface Segregation: Split bloated contracts into lean client-specific interfaces
        
          
        
    - [ ] Dependency Inversion: Inject service interfaces instead of concrete classes
        
          
        
- [ ] **Low-Level Design (LLD) Problem Sets** `#interview` `#dsa`
    
      
    - [ ] **Parking Lot System**
        
          
        - [ ] Design multi-floor slot allocation algorithms (Compact, Large, EV)
            
              
            
        - [ ] Implement polymorphic ticketing and dynamic fee calculation strategies
            
              
            
        - [ ] Ensure thread-safe slot reservation under concurrent entry points
            
              
            
    - [ ] **Rate Limiter**
        
          
        - [ ] Implement the Token Bucket algorithm using atomic counters
            
              
            
        - [ ] Implement the Leaky Bucket algorithm with queue buffers
            
              
            
        - [ ] Implement the Sliding Window Log algorithm to prevent boundary spikes
            
              
            
    - [ ] **Elevator Control System**
        
          
        - [ ] Model internal elevator state transitions (Idle, Moving Up, Moving Down)
            
              
            
        - [ ] Implement SCAN/LOOK request dispatching algorithms
            
              
            
        - [ ] Manage concurrent pickup/dropoff requests across multiple elevator cars
            
              
            

## 3. JVM Architecture & Memory Management

- [ ] **Stack vs Heap Internals** `#interview`
    
      
    - [ ] Diagram thread-isolated stack frames (local variable table, operand stack, frame data)
        
          
        
    - [ ] Trace heap allocation regions and analyze thread contention
        
          
        
    - [ ] Diagnose `StackOverflowError` vs. `OutOfMemoryError: Java heap space`
        
          
        
- [ ] **Metaspace Mechanics** `#interview`
    
      
    - [ ] Trace dynamic off-heap native class metadata storage
        
          
        
    - [ ] Monitor Metaspace expansion and tune `-XX:MaxMetaspaceSize`
        
          
        
    - [ ] Diagnose classloader leak-induced `OutOfMemoryError: Metaspace`
        
          
        
- [ ] **ClassLoader Subsystem** `#interview` `#spring`
    
      
    - [ ] Trace parent-first delegation across Bootstrap, Platform, and Application ClassLoaders
        
          
        
    - [ ] Break delegation order using custom ClassLoaders for plugin architectures
        
          
        
- [ ] **Object Lifecycle & Reference Reachability** `#interview`
    
      
    - [ ] Trace Strong references and Garbage Collection root path traversal
        
          
        
    - [ ] Implement in-memory caches with `SoftReference` for heap-sensitive data
        
          
        
    - [ ] Use `WeakHashMap` with `WeakReference` keys to prevent memory retention
        
          
        
    - [ ] Coordinate `PhantomReference` with `ReferenceQueue` for safe resource cleanup
        
          
        
- [ ] **Generational Garbage Collection** `#interview`
    
      
    - [ ] Trace object allocations in Eden and migrations through Survivor spaces (S0/S1)
        
          
        
    - [ ] Calculate object age thresholds for Tenured/Old generation promotion
        
          
        
    - [ ] Differentiate Minor GC, Major GC, and Full GC impact
        
          
        
- [ ] **Production GC Collectors** `#interview`
    
      
    - [ ] Tune G1GC region-based partitioning, humongous objects, and pause target flags
        
          
        
    - [ ] Profile ZGC colored pointers and load barriers for sub-millisecond pauses
        
          
        
    - [ ] Contrast Shenandoah concurrent evacuation with G1GC mechanics
        
          
        
- [ ] **Memory Leak Patterns & Detection** `#interview` `#spring`
    
      
    - [ ] Reproduce memory leaks from unbounded static collections
        
          
        
    - [ ] Track unclosed I/O streams and unreleased database connections
        
          
        
    - [ ] Clear unremoved `ThreadLocal` variables in pooled container threads
        
          
        
    - [ ] Analyze heap dumps (`.hprof`) via Eclipse MAT or JProfiler
        
          
        
- [ ] **Essential JVM Tuning Flags** `#interview` `#misc`
    
      
    - [ ] Configure `-Xms` and `-Xmx` to identical bounds to eliminate resizing pauses
        
          
        
    - [ ] Apply `-XX:+UseG1GC`, `-XX:+HeapDumpOnOutOfMemoryError`, and `-XX:HeapDumpPath`
        
          
        

## 4. Exception Handling

- [ ] **Exception Hierarchy Architecture** `#interview`
    
      
    - [ ] Map hierarchy: `Throwable` $\to$ `Error` vs. `Exception` $\to$ `RuntimeException`
        
          
        
    - [ ] Audit application code to ensure fatal `Error` types are never caught
        
          
        
- [ ] **Checked vs Unchecked Strategy** `#interview` `#spring`
    
      
    - [ ] Apply checked exceptions for recoverable conditions requiring caller intervention
        
          
        
    - [ ] Apply unchecked runtime exceptions for programmatic defects and contract breaches
        
          
        
- [ ] **Execution Order & Finally Blocks** `#interview`
    
      
    - [ ] Trace return statement precedence inside `try`, `catch`, and `finally`
        
          
        
    - [ ] Verify execution blocks where `finally` is suppressed (e.g., `System.exit()`)
        
          
        
- [ ] **Try-With-Resources (ARM)** `#interview`
    
      
    - [ ] Implement the `AutoCloseable` interface on custom resources
        
          
        
    - [ ] Inspect reverse closure order of declared resources
        
          
        
    - [ ] Retrieve suppressed exceptions using `Throwable.getSuppressed()`
        
          
        
- [ ] **Exception Chaining** `#interview` `#spring`
    
      
    - [ ] Wrap low-level SQL/IO exceptions inside high-level domain exceptions
        
          
        
    - [ ] Preserve root cause stack traces using `initCause()` and cause-accepting constructors
        
          
        
- [ ] **Custom Exception Design** `#interview` `#spring`
    
      
    - [ ] Build contextual exceptions containing error codes, resource IDs, and timestamps
        
          
        
    - [ ] Extend `RuntimeException` for clean propagation to global REST advice handlers
        
          
        
- [ ] **Multi-Catch & Union Types** `#interview` `#misc`
    
      
    - [ ] Consolidate identical catch logic using `catch (IOException | SQLException e)`
        
          
        
    - [ ] Verify that multi-catch reference variables are implicitly `final`
        
          
        

## 5. Generics & Type Erasure

- [ ] **Generic Classes, Interfaces, and Methods** `#interview`
    
      
    - [ ] Write parameterized classes, bounded interfaces, and standalone generic methods
        
          
        
    - [ ] Apply standard naming conventions (`T`, `E`, `K`, `V`, `U`)
        
          
        
- [ ] **Bounded Wildcards** `#interview` `#dsa`
    
      
    - [ ] Enforce upper bounds (`? extends T`) for read-only producer collections
        
          
        
    - [ ] Enforce lower bounds (`? super T`) for write-enabled consumer collections
        
          
        
- [ ] **The PECS Principle** `#interview` `#misc`
    
      
    - [ ] Implement collection utility methods using Producer-Extends, Consumer-Super
        
          
        
    - [ ] Analyze the contract behind `Collections.copy(List<? super T> dest, List<? extends T> src)`
        
          
        
- [ ] **Type Erasure Under The Hood** `#interview`
    
      
    - [ ] Decompile generics into bytecode to inspect bounding type replacements
        
          
        
    - [ ] Inspect synthetic bridge methods generated to preserve polymorphism
        
          
        
- [ ] **Generic Method Inference** `#interview` `#misc`
    
      
    - [ ] Trace compiler type inference in nested generic invocations
        
          
        
    - [ ] Apply explicit type witness syntax (`Collections.<String>emptyList()`)
        
          
        
- [ ] **Heap Pollution & Compiler Warnings** `#interview` `#misc`
    
      
    - [ ] Reproduce heap pollution by mixing raw types with parameterized generics
        
          
        
    - [ ] Evaluate safe usage of `@SuppressWarnings("unchecked")` and `@SafeVarargs`
        
          
        
- [ ] **Reifiable vs Non-Reifiable Types** `#interview` `#misc`
    
      
    - [ ] Analyze compile failure when attempting array creation of generic types (`new T[]`)
        
          
        
    - [ ] Implement generic arrays safely using `Array.newInstance()` or explicit casts
        
          
        

## 6. Collections Framework Internals

- [ ] **ArrayList Architecture** `#interview` `#dsa`
    
      
    - [ ] Trace internal `Object[] elementData` dynamic resizing by 1.5×
        
          
        
    - [ ] Optimize mass additions using `ensureCapacity()` to minimize array copies
        
          
        
    - [ ] Calculate time complexities: $O(1)$ amortized append, $O(n)$ positional insertion/deletion
        
          
        
- [ ] **LinkedList Realities** `#interview` `#dsa`
    
      
    - [ ] Trace doubly-linked `Node<E>` pointers (`prev`, `next`, `item`)
        
          
        
    - [ ] Measure cache misses and memory overhead compared to continuous arrays
        
          
        
- [ ] **ArrayDeque as Preferred Buffer** `#interview` `#dsa` `#misc`
    
      
    - [ ] Trace circular array head/tail pointer wrapping using bitwise masks
        
          
        
    - [ ] Use `ArrayDeque` instead of `Stack` and `LinkedList` for LIFO/FIFO operations
        
          
        
- [ ] **PriorityQueue Internals** `#interview` `#dsa` `#misc`
    
      
    - [ ] Trace array-based binary min-heap representation and bubble-up/down logic
        
          
        
    - [ ] Configure custom `Comparator` instances for max-heaps and multi-field sorting
        
          
        
- [ ] **HashMap Internals** `#interview` `#dsa`
    
      
    - [ ] Trace perturbation function: `(h = key.hashCode()) ^ (h >>> 16)`
        
          
        
    - [ ] Map bucket index via bitwise masking: `index = (n - 1) & hash`
        
          
        
    - [ ] Trace treeification (threshold 8, min table capacity 64) and untreeification (threshold 6)
        
          
        
    - [ ] Trace resizing: 2× capacity expansion, rehashing, and table transfers
        
          
        
- [ ] **LinkedHashMap Mechanics** `#interview` `#dsa` `#misc`
    
      
    - [ ] Trace doubly-linked list ordering superimposed across hash buckets
        
          
        
    - [ ] Implement a bounded LRU cache by overriding `removeEldestEntry()`
        
          
        
- [ ] **TreeMap & NavigableMap** `#interview` `#dsa`
    
      
    - [ ] Trace Red-Black tree self-balancing rotations and color-flip invariants
        
          
        
    - [ ] Query boundaries using `floorKey()`, `ceilingKey()`, `higherEntry()`, and `subMap()`
        
          
        
- [ ] **Set Implementations** `#interview` `#dsa`
    
      
    - [ ] Map `HashSet` operations directly to backing `HashMap` dummy values
        
          
        
    - [ ] Maintain deterministic iteration order with `LinkedHashSet`
        
          
        
    - [ ] Enforce logarithmic sorted unique invariants with `TreeSet`
        
          
        
- [ ] **ConcurrentHashMap Architecture** `#interview` `#spring`
    
      
    - [ ] Trace bucket-level synchronization: CAS for empty nodes, `synchronized` on bin heads
        
          
        
    - [ ] Perform thread-safe modifications using `computeIfAbsent()`
        
          
        
    - [ ] Iterate safely using weakly-consistent, non-blocking iterators
        
          
        
- [ ] **Fail-Fast vs Fail-Safe Iteration** `#interview`
    
      
    - [ ] Trace structural mutation tracking with internal `modCount` counters
        
          
        
    - [ ] Handle `ConcurrentModificationException` risks
        
          
        
    - [ ] Analyze snapshot iteration in `CopyOnWriteArrayList` and its write-penalty costs
        
          
        
- [ ] **Comparable vs Comparator** `#interview` `#dsa`
    
      
    - [ ] Implement natural domain ordering with `Comparable<T>`
        
          
        
    - [ ] Compose multi-tier sorting pipelines using `Comparator.comparing().thenComparing()`
        
          
        
- [ ] **Custom Comparators for DSA** `#interview` `#dsa` `#misc`
    
      
    - [ ] Prevent integer subtraction overflow bugs using `Integer.compare(a, b)`
        
          
        
    - [ ] Sort multi-dimensional primitive arrays (`int[][]`) for geometric and interval logic
        
          
        
- [ ] **Unmodifiable vs Immutable Collections** `#interview`
    
      
    - [ ] Inspect read-only wrapper views (`Collections.unmodifiableList()`) against backing mutation
        
          
        
    - [ ] Construct deeply immutable collections using `List.of()`, `Set.of()`, and `Map.of()`
        
          
        
- [ ] **Custom Iterators** `#interview` `#misc`
    
      
    - [ ] Implement `Iterable<T>` and `Iterator<T>` on custom tree and graph structures
        
          
        
    - [ ] Enforce the `hasNext()`, `next()`, and safe `iterator.remove()` contract
        
          
        

## 7. Functional Programming & Streams API

- [ ] **Lambda Expressions & Scope Capture** `#interview` `#spring`
    
      
    - [ ] Enforce effectively final constraints on local captured variables
        
          
        
    - [ ] Resolve lexical scope and `this` reference binding inside lambdas
        
          
        
- [ ] **Core Functional Interfaces** `#interview` `#spring`
    
      
    - [ ] Implement standard functional primitives: `Predicate<T>`, `Function<T, R>`, `Consumer<T>`, `Supplier<T>`
        
          
        
    - [ ] Compose functional chains using `andThen()`, `compose()`, and `and()`
        
          
        
- [ ] **Method References** `#interview` `#misc`
    
      
    - [ ] Static method references (`Math::max`)
        
          
        
    - [ ] Bound instance method references (`System.out::println`)
        
          
        
    - [ ] Unbound instance method references (`String::toUpperCase`)
        
          
        
    - [ ] Constructor references (`ArrayList::new`)
        
          
        
- [ ] **Stream Pipeline Mechanics** `#interview` `#spring`
    
      
    - [ ] Trace lazy evaluation of intermediate operations until terminal execution
        
          
        
    - [ ] Handle `IllegalStateException` on attempts to consume closed streams
        
          
        
- [ ] **Intermediate Stream Operations** `#interview` `#dsa`
    
      
    - [ ] Transform data using `map()`, `filter()`, and `peek()`
        
          
        
    - [ ] Flatten nested data structures using `flatMap()`
        
          
        
    - [ ] Handle stateful intermediate overheads in `sorted()` and `distinct()`
        
          
        
- [ ] **Terminal Stream Operations** `#interview` `#dsa`
    
      
    - [ ] Aggregate values using `reduce(identity, accumulator, combiner)`
        
          
        
    - [ ] Apply short-circuit terminal operations (`findFirst()`, `anyMatch()`, `allMatch()`)
        
          
        
- [ ] **Advanced Collectors** `#interview` `#spring`
    
      
    - [ ] Partition and group data structures using `Collectors.groupingBy()`
        
          
        
    - [ ] Aggregate complex summaries using `Collectors.toMap()`, `joining()`, and `summarizingInt()`
        
          
        
- [ ] **Parallel Stream Processing** `#interview`
    
      
    - [ ] Profile thread distribution over the shared `ForkJoinPool.commonPool()`
        
          
        
    - [ ] Eliminate race conditions from shared mutable state in parallel streams
        
          
        
- [ ] **Optional Deep-Dive** `#interview` `#spring` `#misc`
    
      
    - [ ] Compare eager evaluation of `orElse()` against lazy evaluation of `orElseGet()`
        
          
        
    - [ ] Chain transforms using `Optional.map()`, `flatMap()`, and `filter()`
        
          
        
    - [ ] Replace anti-pattern `optional.get()` with `orElseThrow()`
        
          
        

## 8. Multithreading & High Concurrency

- [ ] **Thread Lifecycle & State Transitions** `#interview`
    
      
    - [ ] Map lifecycle states: `NEW`, `RUNNABLE`, `BLOCKED`, `WAITING`, `TIMED_WAITING`, `TERMINATED`
        
          
        
    - [ ] Distinguish monitor contention (`BLOCKED`) from conditional waiting (`WAITING`)
        
          
        
- [ ] **Runnable vs Callable Execution** `#interview`
    
      
    - [ ] Wrap tasks returning results and throwing checked exceptions with `Callable<V>`
        
          
        
    - [ ] Manage asynchronous execution and non-blocking checks with `Future<V>`
        
          
        
- [ ] **Executor Framework & Thread Pools** `#interview` `#spring`
    
      
    - [ ] Manage thread pool lifecycles via `ExecutorService`
        
          
        
    - [ ] Contrast graceful draining (`shutdown()`) with immediate interruption (`shutdownNow()`)
        
          
        
- [ ] **ThreadPoolExecutor Tuning** `#interview` `#spring` `#misc`
    
      
    - [ ] Configure `corePoolSize`, `maximumPoolSize`, and `keepAliveTime`
        
          
        
    - [ ] Select appropriate work queues: `ArrayBlockingQueue`, `LinkedBlockingQueue`, `SynchronousQueue`
        
          
        
    - [ ] Handle saturation with rejection policies: `AbortPolicy`, `CallerRunsPolicy`, `DiscardOldestPolicy`
        
          
        
- [ ] **Intrinsic Locking (Synchronized)** `#interview`
    
      
    - [ ] Trace object header Mark Word lock inflation (Biased $\to$ Lightweight $\to$ Heavyweight)
        
          
        
    - [ ] Contrast instance synchronization locks with static class-level monitors
        
          
        
- [ ] **Volatile & The Java Memory Model (JMM)** `#interview`
    
      
    - [ ] Enforce visibility across CPU caches via memory barriers
        
          
        
    - [ ] Prevent instruction reordering optimizations by the compiler and CPU
        
          
        
    - [ ] Demonstrate non-atomicity of compound operations (`count++`) on volatile fields
        
          
        
- [ ] **Double-Checked Locking Singleton** `#interview` `#misc`
    
      
    - [ ] Implement thread-safe DCL using `volatile` to prevent partially initialized reads
        
          
        
    - [ ] Alternative: Implement the Bill Pugh Static Helper Holder pattern
        
          
        
- [ ] **Explicit Locks (ReentrantLock)** `#interview`
    
      
    - [ ] Implement non-blocking locks with `tryLock()` and acquisition timeouts
        
          
        
    - [ ] Configure lock fairness policies and manage explicit `lock()` / `unlock()` blocks
        
          
        
    - [ ] Coordinate thread signaling with multiple `Condition` variables (`await()`, `signal()`)
        
          
        
- [ ] **ReadWriteLock & StampedLock** `#interview` `#misc`
    
      
    - [ ] Maximize read throughput on read-heavy structures with `ReentrantReadWriteLock`
        
          
        
    - [ ] Implement optimistic read validations with `StampedLock` to eliminate write starvation
        
          
        
- [ ] **Atomic Variables & Lock-Free Programming** `#interview`
    
      
    - [ ] Trace Compare-And-Swap (CAS) hardware instructions (`AtomicInteger`, `AtomicReference`)
        
          
        
    - [ ] Implement optimistic lock-free loops using `compareAndSet()`
        
          
        
- [ ] **ThreadLocal Storage** `#interview` `#misc`
    
      
    - [ ] Isolate execution context per thread (e.g., MDC logging, transaction contexts)
        
          
        
    - [ ] Eliminate thread pool memory leaks by executing `ThreadLocal.remove()` in `finally` blocks
        
          
        
- [ ] **Synchronization Utilities** `#interview` `#misc`
    
      
    - [ ] Implement one-shot countdown latches using `CountDownLatch`
        
          
        
    - [ ] Synchronize cyclical rendezvous phases using `CyclicBarrier`
        
          
        
    - [ ] Throttle resource access and rate limits using `Semaphore`
        
          
        
- [ ] **Asynchronous Pipelines (CompletableFuture)** `#interview` `#spring`
    
      
    - [ ] Chain dependencies using `thenApply()`, `thenCompose()`, and `thenCombine()`
        
          
        
    - [ ] Handle errors asynchronously using `exceptionally()` and `handle()`
        
          
        
    - [ ] Coordinate multiple async operations using `CompletableFuture.allOf()`
        
          
        
- [ ] **Virtual Threads (Project Loom - Java 21+)** `#interview` `#misc`
    
      
    - [ ] Mount lightweight virtual threads to underlying carrier OS worker threads
        
          
        
    - [ ] Eliminate thread pool wrapping for virtual thread instances
        
          
        
    - [ ] Benchmark high-throughput blocking I/O against traditional thread architectures
        
          
        
- [ ] **Concurrency Anti-Patterns & Diagnostics** `#interview` `#misc`
    
      
    - [ ] Trace Coffman conditions leading to thread deadlocks
        
          
        
    - [ ] Diagnose lock contention and live deadlocks using `jcmd`, `jstack`, and visual profilers
        
          
        
    - [ ] Debug livelock, priority inversion, and thread starvation scenarios
        
          
        

## 9. I/O, Serialization & Networking Basics

- [ ] **Stream I/O Architecture** `#interview`
    
      
    - [ ] Contrast byte streams (`InputStream`/`OutputStream`) vs. character streams (`Reader`/`Writer`)
        
          
        
    - [ ] Wrap low-level streams in `BufferedInputStream` and `BufferedReader` for performance
        
          
        
- [ ] **Non-Blocking I/O (java.nio)** `#interview`
    
      
    - [ ] Manage data flow across `Channels`, `Buffers` (capacity, position, limit), and `Selectors`
        
          
        
    - [ ] Build single-threaded multiplexed servers handling concurrent open sockets
        
          
        
- [ ] **Java Native Serialization** `#interview`
    
      
    - [ ] Implement `Serializable` and manage schema versioning with `serialVersionUID`
        
          
        
    - [ ] Suppress sensitive field persistence using the `transient` modifier
        
          
        
    - [ ] Identify security vulnerabilities inherent in native Java deserialization
        
          
        
- [ ] **Modern File System NIO.2** `#interview` `#misc`
    
      
    - [ ] Manipulate file system paths safely using `Path` and `Files`
        
          
        
    - [ ] Traverse deeply nested directory structures using `Files.walk()` streams
        
          
        

## 10. Reflection, Annotations & Framework Foundations

- [ ] **Reflection API Internals** `#interview` `#spring`
    
      
    - [ ] Inspect classes, methods, and private fields via `Class<?>` metadata
        
          
        
    - [ ] Bypass access modifiers with `field.setAccessible(true)`
        
          
        
    - [ ] Measure JIT optimization penalties and reflection invocation overhead
        
          
        
- [ ] **Custom Annotation Architecture** `#interview` `#spring`
    
      
    - [ ] Build custom annotations using `@Retention(RetentionPolicy.RUNTIME)` and `@Target`
        
          
        
    - [ ] Process custom annotation metadata dynamically at runtime
        
          
        
- [ ] **Dynamic Proxies & Bytecode Generation** `#interview` `#spring` `#misc`
    
      
    - [ ] Implement interface-based dynamic proxies via `Proxy.newProxyInstance()`
        
          
        
    - [ ] Compare JDK dynamic proxies with class-intercepting CGLIB / ByteBuddy proxies
        
          
        
    - [ ] Trace runtime proxy generation used in Spring `@Transactional` and AOP
        
          
        
# PART 2 
## 11. Design Principles & GoF Patterns

- [ ] **Creational Design Patterns** `#interview` `#spring`
    
      
    - [ ] Build thread-safe Singletons (Enum, Bill Pugh, Double-Checked Locking)
        
          
        
    - [ ] Implement Factory Method and Abstract Factory to decouple creation from usage
        
          
        
    - [ ] Implement the Builder pattern with validation steps for complex domain entities
        
          
        
- [ ] **Structural Design Patterns** `#interview` `#spring`
    
      
    - [ ] Implement Adapter to bridge incompatible legacy interfaces
        
          
        
    - [ ] Implement Decorator dynamically wrapping behaviors (e.g., Java I/O streams)
        
          
        
    - [ ] Implement Proxy to handle lazy loading, security checks, and transaction management
        
          
        
    - [ ] Implement Facade to expose unified simplified interfaces over complex sub-systems
        
          
        
- [ ] **Behavioral Design Patterns** `#interview` `#spring`
    
      
    - [ ] Implement Strategy to swap runtime algorithms and eliminate branching `if-else` blocks
        
          
        
    - [ ] Implement Observer for decoupled event notification models
        
          
        
    - [ ] Implement Template Method defining invariant algorithm skeletons with abstract hooks
        
          
        
- [ ] **Dependency Injection Pattern** `#interview` `#spring` `#misc`
    
      
    - [ ] Compare Constructor Injection vs. Setter Injection vs. Field Injection
        
          
        
    - [ ] Trace why constructor injection is preferred (immutability, mockability, cyclical detection)
        
          
        

## 12. Database Engineering, Persistence & ORM

- [ ] **Relational Database Internals & PostgreSQL** `#interview` `#spring`
    
      
    - [ ] Map write paths: Write-Ahead Logging (WAL), page buffers, and dirty page flushing
        
          
        
    - [ ] Trace Multi-Version Concurrency Control (MVCC) isolation and vacuuming mechanics
        
          
        
    - [ ] Compare transaction isolation levels: `READ UNCOMMITTED`, `READ COMMITTED`, `REPEATABLE READ`, `SERIALIZABLE`
        
          
        
    - [ ] Verify isolation anomalies: Dirty Reads, Non-Repeatable Reads, Phantom Reads, Serialization Failures
        
          
        
- [ ] **Advanced SQL & Indexing** `#interview` `#spring` `#misc`
    
      
    - [ ] Write complex queries using subqueries, Common Table Expressions (CTEs), and Window Functions (`ROW_NUMBER()`, `RANK()`, `PARTITION BY`)
        
          
        
    - [ ] Trace B-Tree index node splits, traversal overhead, and range scan performance
        
          
        
    - [ ] Enforce the Leftmost Prefix Rule on composite index designs
        
          
        
    - [ ] Optimize queries via Covering Indexes to eliminate table heap fetches
        
          
        
    - [ ] Profile query execution plans using `EXPLAIN (ANALYZE, BUFFERS)`
        
          
        
- [ ] **JDBC & Connection Pool Tuning** `#interview` `#spring`
    
      
    - [ ] Trace JDBC lifecycle: `DriverManager`, `Connection`, `PreparedStatement`, `ResultSet`
        
          
        
    - [ ] Explain plan caching and SQL injection prevention in `PreparedStatement`
        
          
        
    - [ ] Tune HikariCP: `maximumPoolSize`, `minimumIdle`, `connectionTimeout`, `leakDetectionThreshold`
        
          
        
- [ ] **JPA & Hibernate Entity Lifecycle** `#interview` `#spring`
    
      
    - [ ] Trace entity states: Transient, Managed/Persistent, Detached, Removed
        
          
        
    - [ ] Profile dirty-checking cascades on managed entities during transaction flush
        
          
        
- [ ] **JPA Relationship Mappings & Loading** `#interview` `#spring`
    
      
    - [ ] Configure `@OneToOne`, `@OneToMany`, `@ManyToOne`, and `@ManyToMany` mappings
        
          
        
    - [ ] Set bidirectional ownership sides correctly using `mappedBy`
        
          
        
    - [ ] Contrast Lazy loading proxies (`ByteBuddy`) vs. Eager fetch strategies
        
          
        
    - [ ] Diagnose and prevent runtime `LazyInitializationException`
        
          
        
- [ ] **The N+1 Query Problem & Remediation** `#interview` `#spring` `#misc`
    
      
    - [ ] Reproduce N+1 query explosions via automated SQL execution logs
        
          
        
    - [ ] Fix N+1 queries using explicit `JOIN FETCH` queries
        
          
        
    - [ ] Fix N+1 queries using Hibernate `@BatchSize(size = 25)`
        
          
        
    - [ ] Fix N+1 queries using dynamic JPA `@EntityGraph` definitions
        
          
        
- [ ] **Transaction Management** `#interview` `#spring`
    
      
    - [ ] Configure Spring `@Transactional` boundaries and proxy interceptors
        
          
        
    - [ ] Trace transaction propagation rules: `REQUIRED`, `REQUIRES_NEW`, `NESTED`
        
          
        
    - [ ] Configure rollback rules for checked vs. unchecked exceptions
        
          
        
- [ ] **Distributed Caching (Redis)** `#interview` `#spring` `#misc`
    
      
    - [ ] Model data structures: Strings, Hashes, Lists, Sets, and Sorted Sets (ZSET)
        
          
        
    - [ ] Implement caching topologies: Cache-Aside, Write-Through, and Write-Behind
        
          
        
    - [ ] Mitigate cache hazards: Cache Stampede (mutex locking), Penetration (Bloom filters), Avalanche (TTL jitter)
        
          
        
    - [ ] Configure Redis eviction policies (`allkeys-lru`, `volatile-lru`) and evaluate distributed locking (Redlock)
        
          
        

## 13. Spring Boot, Web & Microservices Ecosystem

- [ ] **Core Spring Framework & IoC Container** `#interview` `#spring`
    
      
    - [ ] Differentiate `BeanFactory` from `ApplicationContext`
        
          
        
    - [ ] Trace Bean Lifecycle: Instantiation $\to$ Property Injection $\to$ Aware Interfaces $\to$ BeanPostProcessor $\to$ `@PostConstruct` / InitializingBean $\to$ Running $\to$ `@PreDestroy`
        
          
        
    - [ ] Manage Bean Scopes: `singleton`, `prototype`, `request`, `session`
        
          
        
    - [ ] Resolve prototype-in-singleton injection mismatch using `ObjectProvider` or scoped proxies
        
          
        
    - [ ] Differentiate component annotations: `@Component`, `@Service`, `@Repository`, `@Controller`, `@RestController`
        
          
        
- [ ] **Spring Boot Mechanics & Configuration** `#interview` `#spring`
    
      
    - [ ] Trace auto-configuration loading from `AutoConfiguration.imports`
        
          
        
    - [ ] Apply conditional annotations: `@ConditionalOnProperty`, `@ConditionalOnClass`, `@ConditionalOnMissingBean`
        
          
        
    - [ ] Bind external configurations using type-safe `@ConfigurationProperties`
        
          
        
    - [ ] Monitor health, metrics, and thread dumps using Spring Boot Actuator
        
          
        
- [ ] **REST API Architecture & Web MVC** `#interview` `#spring`
    
      
    - [ ] Trace request dispatching through `DispatcherServlet`, `HandlerMapping`, and `HandlerAdapter`
        
          
        
    - [ ] Validate HTTP semantics: GET, POST, PUT, PATCH, DELETE idempotency and safety rules
        
          
        
    - [ ] Return appropriate HTTP response statuses: 200, 201, 204, 400, 401, 403, 404, 409, 500
        
          
        
    - [ ] Enforce payload constraints using Jakarta Bean Validation (`@Valid`, `@NotNull`, `@Size`)
        
          
        
    - [ ] Catch exceptions centrally using `@ControllerAdvice` and `@ExceptionHandler` emitting RFC 7807 payloads
        
          
        
    - [ ] Configure JSON serialization with Jackson: handle cycles via `@JsonIgnore` / `@JsonBackReference`
        
          
        
- [ ] **Spring Security & Authentication Architecture** `#interview` `#spring`
    
      
    - [ ] Trace request handling through the `SecurityFilterChain`
        
          
        
    - [ ] Extract credentials into `Authentication` tokens managed by `SecurityContextHolder`
        
          
        
    - [ ] Implement stateless JWT authentication: cryptographic signing, expiration, and refresh rotations
        
          
        
    - [ ] Enforce Method-Level Authorization using `@PreAuthorize("hasRole('ADMIN')")`
        
          
        
    - [ ] Configure CORS filters and evaluate CSRF protections in stateless APIs
        
          
        
- [ ] **Microservices Design Patterns** `#interview` `#spring` `#misc`
    
      
    - [ ] Define microservice boundaries using Domain-Driven Design (DDD) bounded contexts
        
          
        
    - [ ] Implement API Gateway patterns: reverse routing, SSL termination, and rate-limiting
        
          
        
    - [ ] Configure Client-Side and Server-Side Service Discovery models
        
          
        
    - [ ] Implement Resilience Patterns using Resilience4j: Circuit Breaker, Retry with Backoff, Bulkhead Isolation
        
          
        
    - [ ] Coordinate distributed transactions using the Saga Pattern (Choreography vs. Orchestration)
        
          
        
    - [ ] Prevent data loss in event-driven systems using the Transactional Outbox Pattern
        
          
        
- [ ] **Spring AI Integration** `#interview` `#spring` `#misc`
    
      
    - [ ] Connect Spring applications to LLM endpoints using `ChatClient` and `ChatModel`
        
          
        
    - [ ] Generate typed JSON payloads by parsing model outputs directly into Java records/POJOs
        
          
        
    - [ ] Build contextual prompt pipelines supporting Retrieval-Augmented Generation (RAG)
        
          
        

## 14. Testing & Software Quality

- [ ] **Unit Testing with JUnit 5** `#interview` `#spring`
    
      
    - [ ] Manage test lifecycle phases: `@BeforeAll`, `@BeforeEach`, `@AfterEach`, `@AfterAll`
        
          
        
    - [ ] Write fluent, expressive state assertions using AssertJ
        
          
        
    - [ ] Organize complex test suites hierarchically using `@Nested` test classes
        
          
        
    - [ ] Execute data-driven test scenarios using `@ParameterizedTest` (`@ValueSource`, `@CsvSource`)
        
          
        
- [ ] **Mocking Frameworks (Mockito)** `#interview` `#spring`
    
      
    - [ ] Isolate dependencies using `@Mock`, `@InjectMocks`, and partial mock wrappers (`@Spy`)
        
          
        
    - [ ] Stub behavior using `when().thenReturn()` and `doThrow()`
        
          
        
    - [ ] Verify execution counts (`verify(mock, times(1))`) and capture arguments with `ArgumentCaptor`
        
          
        
    - [ ] Differentiate test doubles: Dummies, Stubs, Mocks, Spies, and Fakes
        
          
        
- [ ] **Spring Integration Testing** `#interview` `#spring`
    
      
    - [ ] Run full-context integration tests using `@SpringBootTest`
        
          
        
    - [ ] Isolate web-layer controllers using sliced `@WebMvcTest` configurations
        
          
        
    - [ ] Isolate database repositories using sliced `@DataJpaTest` configurations
        
          
        
    - [ ] Spin up real PostgreSQL and Redis test instances dynamically using Testcontainers
        
          
        
- [ ] **Methodologies & Quality Metrics** `#interview` `#misc`
    
      
    - [ ] Apply the Test-Driven Development (TDD) cycle: Red $\to$ Green $\to$ Refactor
        
          
        
    - [ ] Profile branch and line test coverage targets using JaCoCo
        
          
        

## 15. DevOps, Infrastructure & System Delivery

- [ ] **Build Automation Tools (Maven & Gradle)** `#interview` `#misc`
    
      
    - [ ] Step through Maven lifecycle phases: `validate`, `compile`, `test`, `package`, `verify`, `install`, `deploy`
        
          
        
    - [ ] Manage dependency scopes: `compile`, `provided`, `runtime`, `test`
        
          
        
    - [ ] Resolve transitive dependency version conflicts via nearest-wins rules and `<exclusions>`
        
          
        
    - [ ] Optimize incremental builds and task caching using Gradle (`implementation` vs. `api`)
        
          
        
- [ ] **Version Control Systems (Git)** `#interview` `#misc`
    
      
    - [ ] Compare branching strategies: Git Flow vs. Trunk-Based Development
        
          
        
    - [ ] Contrast history preservation: `git merge` (3-way / fast-forward) vs. linear `git rebase`
        
          
        
    - [ ] Clean up commit histories interactively using `git rebase -i`
        
          
        
    - [ ] Apply individual commits across branches using `git cherry-pick`
        
          
        
    - [ ] Resolve merge conflict markers and manage state with `git stash`
        
          
        
    - [ ] Contrast state modifications: `git reset` (soft, mixed, hard) vs. safe `git revert`
        
          
        
- [ ] **Containerization (Docker)** `#interview` `#misc`
    
      
    - [ ] Trace Linux isolation primitives: namespaces, cgroups, and layered filesystems
        
          
        
    - [ ] Write optimized multi-stage Dockerfiles for lean, minimal Java runtime images
        
          
        
    - [ ] Enforce container security by running processes under non-root users
        
          
        
    - [ ] Configure network modes (Bridge, Host) and manage persistent data with Named Volumes
        
          
        
    - [ ] Orchestrate multi-container local environments using Docker Compose
        
          
        
- [ ] **Cloud Architecture (AWS)** `#interview` `#misc`
    
      
    - [ ] Provision compute instances via EC2 and containerized workloads via ECS with Fargate
        
          
        
    - [ ] Provision managed persistence via RDS PostgreSQL (multi-AZ replication, automated snapshots)
        
          
        
    - [ ] Store application assets securely in S3 using pre-signed URLs and lifecycle policies
        
          
        
    - [ ] Construct cloud networks: VPC, Public/Private subnets, Route Tables, Internet Gateways, NAT Gateways
        
          
        
    - [ ] Configure firewall rules with Security Groups vs. Network ACLs (NACLs)
        
          
        
    - [ ] Enforce Principle of Least Privilege using IAM roles and fine-grained policies
        
          
        
- [ ] **Agile Engineering & Delivery Workflows** `#interview` `#misc`
    
      
    - [ ] Contrast delivery models: Scrum (fixed sprints, velocity) vs. Kanban (continuous flow, WIP limits)
        
          
        
    - [ ] Participate in engineering ceremonies: Sprint Planning, Daily Standups, Reviews, Retrospectives
        
          
        
    - [ ] Estimate engineering tasks using Story Points, Planning Poker, and manage Jira workflows
        
          
        

## 16. Modern Frontend Integration

- [ ] **React & Next.js Core Concepts** `#misc`
    
      
    - [ ] Manage immutable component state and side-effects via hooks (`useState`, `useEffect`)
        
          
        
    - [ ] Optimize performance using `useCallback`, `useMemo`, and persistent `useRef`
        
          
        
    - [ ] Architecture design: React Server Components (RSC) vs. Client Components (`'use client'`)
        
          
        
    - [ ] Manage client data caches and optimistic updates using TanStack Query (React Query) / SWR
        
          
        
- [ ] **Component Systems & UI Styling** `#misc`
    
      
    - [ ] Build responsive, utility-first interfaces using Tailwind CSS
        
          
        
    - [ ] Assemble accessible UI design systems using `shadcn/ui` (Radix primitives)
        
          
        
    - [ ] Enforce route security on protected pages using Next.js Edge Middleware
        
          
        
    - [ ] Store authenticated JWT tokens securely inside HTTP-only cookies
        
          
        

## 17. System Design & Distributed Architecture (HLD)

- [ ] **Distributed Foundations & Tradeoffs** `#interview` `#misc`
    
      
    - [ ] Analyze trade-offs in distributed systems using the CAP Theorem and PACELC Theorem
        
          
        
    - [ ] Plan scaling paths: Vertical compute upgrades vs. Horizontal stateless scaling
        
          
        
- [ ] **Traffic Management & Load Balancing** `#interview` `#misc`
    
      
    - [ ] Contrast Layer 4 (Transport/TCP) vs. Layer 7 (Application/HTTP) load balancers
        
          
        
    - [ ] Trace routing algorithms: Round Robin, Weighted Least Connections, Consistent Hashing
        
          
        
- [ ] **Asynchronous Decoupling & Event Brokers** `#interview` `#misc`
    
      
    - [ ] Contrast Point-to-Point message queues vs. Distributed Publish-Subscribe logs
        
          
        
    - [ ] Differentiate delivery semantics: At-Most-Once, At-Least-Once, Exactly-Once
        
          
        
    - [ ] Implement idempotent consumers to guarantee deduplicated message processing
        
          
        
- [ ] **Database Partitioning & High Availability** `#interview` `#misc`
    
      
    - [ ] Configure Read Replicas and address replication lag in read-heavy applications
        
          
        
    - [ ] Partition large data sets horizontally via database sharding strategies (Hash vs. Range)
        
          
        

## 18. Continuous Parallel Tracks

### Track A: Daily Data Structures & Algorithms (DSA)

- [ ] **Striver's A-to-Z DSA Target (250–300 Problems, 2–3 Daily)** `#dsa`
    
      
    - [ ] **Arrays & Hashing:** Two Pointers, Prefix Sums, Kadane's Algorithm, Moore's Voting
        
          
        
    - [ ] **Two Pointers & Sliding Window:** Fixed vs. dynamic windows, substring counts
        
          
        
    - [ ] **Binary Search:** Search space reduction, lower/upper bounds, BS on answer ranges
        
          
        
    - [ ] **Linked Lists:** Fast and slow pointers (Floyd's cycle detection), in-place reversal
        
          
        
    - [ ] **Stacks & Queues:** Next Greater Element using monotonic stacks, LRU Cache
        
          
        
    - [ ] **Trees & BSTs:** Level-order traversals, LCA, height/diameter, validate BST property
        
          
        
    - [ ] **Heaps & Priority Queues:** Top-K frequent elements, merge K sorted lists
        
          
        
    - [ ] **Graphs:** BFS, DFS, Kahn's Topological Sort, Dijkstra's algorithm, Disjoint Set Union (DSU)
        
          
        
    - [ ] **Dynamic Programming:** 1D arrays, 2D grid DP, 0/1 Knapsack variants, Longest Common Subsequence (LCS)
        
          
        

### Track B: The Flagship Production Project

- [ ] **Enterprise Full-Stack Application (Spring Boot + Next.js + PostgreSQL + Redis + Docker + AWS)**
    
      
    - [ ] **Phase 1: Security & Identity Foundation**
        
          
        - [ ] Implement stateless authentication with asymmetric/symmetric JWT in HTTP-only cookies
            
              
            
        - [ ] Implement refresh token rotation and database revocation tables
            
              
            
        - [ ] Configure multi-role authorization checks (Admin, User, Auditor)
            
              
            
    - [ ] **Phase 2: Data Modeling & Persistence Engine**
        
          
        - [ ] Build complex relational schemas with bidirectional JPA entities
            
              
            
        - [ ] Optimize read queries using explicit `@EntityGraph` mappings to eliminate N+1 queries
            
              
            
        - [ ] Configure HikariCP connection pooling and verify zero connection leaks
            
              
            
    - [ ] **Phase 3: High-Performance Caching Layer**
        
          
        - [ ] Implement Cache-Aside workflows for hot read paths using Redis
            
              
            
        - [ ] Apply TTL jitter strategies to prevent cache avalanche failures
            
              
            
    - [ ] **Phase 4: Asynchronous Processing & Micro-Integrations**
        
          
        - [ ] Offload heavy background workloads via Spring `@Async` and custom `ThreadPoolExecutor` configurations
            
              
            
        - [ ] Integrate real-time notification pushes (WebSockets or Server-Sent Events)
            
              
            
        - [ ] Integrate Spring AI endpoints for automated entity processing or summary generation
            
              
            
    - [ ] **Phase 5: Automated Testing & Verification**
        
          
        - [ ] Achieve robust line/branch coverage across domain services using JUnit 5 and Mockito
            
              
            
        - [ ] Implement integration test suites backed by disposable Testcontainers instances
            
              
            
    - [ ] **Phase 6: Containerization & Cloud Deployment**
        
          
        - [ ] Write multi-stage Dockerfiles building lightweight production images
            
              
            
        - [ ] Construct a local development topology using Docker Compose
            
              
            
        - [ ] Provision AWS infrastructure: VPC subnets, RDS PostgreSQL, and ECS Fargate compute
            
              
            
        - [ ] Deploy the Next.js frontend and establish public-facing routing via an Application Load Balance