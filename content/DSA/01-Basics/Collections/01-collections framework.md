In Data Structures and Algorithms (DSA), knowing which Java collection to pick—and how its time complexities and internal data structures work—is essential for writing optimal solutions.

### Hierarchy

![](../../../../attachments/01-collections%20framework-1787980567392.webp)

```
               Iterable<E>
                   │
              Collection<E>
        ┌──────────┼──────────┐
      List<E>    Set<E>    Queue<E> ────── Deque<E>
                            │
                      BlockingQueue<E>

(Standalone Hierarchy)
              Map<K, V>
```

### Key Interfaces & Implementation Classes

#### 1. `List<E>` (Ordered Sequences)

- **`ArrayList<E>`**
    
      
    - **Underlying DS:** Resizable dynamic array (grows by $\approx 50\%$ when capacity is exceeded).
        
          
        
    - **Complexity:** Random access `get(i)`/`set(i)` is $O(1)$; amortized addition `add(e)` is $O(1)$; middle insertion/deletion is $O(n)$.
        
          
        
    - **DSA Use Cases:** Dynamic lists, graph adjacency lists (`List<List<Integer>>`), prefix-sum storage, backtracking result aggregation.
        
          
        
- **`LinkedList<E>`**
    
      
    - **Underlying DS:** Doubly linked list.
        
          
        
    - **Complexity:** $O(1)$ insertions/deletions at the head/tail; $O(n)$ random access.
        
          
        
    - **DSA Note:** In practice, **`ArrayDeque` is strictly preferred over `LinkedList`** for stacks and queues due to lower memory overhead and superior CPU cache locality.
        
          
        

#### 2. `Set<E>` (Unique Elements)

- **`HashSet<E>`**
    
      
    - **Underlying DS:** Hash table (backed internally by a `HashMap`).
        
          
        
    - **Complexity:** Average $O(1)$ for `add()`, `remove()`, and `contains()`.
        
          
        
    - **DSA Use Cases:** Visited sets in BFS/DFS, tracking unique elements in sliding windows, two-sum lookups.
        
          
        
- **`TreeSet<E>`** (implements `NavigableSet<E>`, `SortedSet<E>`)
    
      
    - **Underlying DS:** Self-balancing Red-Black Tree.
        
          
        
    - **Complexity:** $O(\log n)$ for `add()`, `remove()`, and `contains()`.
        
          
        
    - **Key DSA Methods:** `ceiling(e)` ($\ge$), `floor(e)` ($\le$), `higher(e)` ($>$), `lower(e)` ($<$), `first()`, `last()`.
        
          
        
    - **DSA Use Cases:** Range queries, finding nearest smaller/larger elements dynamically, interval overlap problems.
        
          
        
- **`LinkedHashSet<E>`**
    
      
    - **Underlying DS:** Hash table + Doubly linked list running across entries.
        
          
        
    - **Complexity:** $O(1)$ operations with deterministic insertion-order iteration.
        
          
        

#### 3. `Queue<E>` & `Deque<E>` (FIFO, LIFO, & Priority)

- **`ArrayDeque<E>`** (implements `Deque<E>`)
    
      
    - **Underlying DS:** Circular array (resizes automatically).
        
          
        
    - **Complexity:** Amortized $O(1)$ for all end operations (`offerFirst`, `pollFirst`, `offerLast`, `pollLast`).
        
          
        
    - **DSA Use Cases:**
        
          
        - **Queue (FIFO):** Standard BFS queue (`offer()` / `poll()`).
            
              
            
        - **Stack (LIFO):** Monotonic stacks, DFS, valid parenthesis validation (`push()` / `pop()`). _Always use `ArrayDeque` instead of the legacy `java.util.Stack` class (which has synchronization overhead)._
            
              
            
- **`PriorityQueue<E>`** (implements `Queue<E>`)
    
      
    - **Underlying DS:** Binary Min-Heap (default).
        
          
        
    - **Complexity:** $O(\log n)$ for `offer()` (push) and `poll()` (pop); $O(1)$ for `peek()` (top); $O(n)$ for arbitrary element removal.
        
          
        
    - **Max-Heap Syntax:** `new PriorityQueue<>(Collections.reverseOrder())` or `(a, b) -> b - a`.
        
          
        
    - **DSA Use Cases:** Dijkstra's Algorithm, Prim's MST, Top-K elements, Huffman Coding, Median from Data Stream (dual heaps).
        
          
        

#### 4. `Map<K, V>` (Key-Value Associations)

- **`HashMap<K, V>`**
    
      
    - **Underlying DS:** Array of buckets (Hash table). Uses linked lists for collisions, which _treeify_ into Red-Black Trees when bucket size exceeds 8.
        
          
        
    - **Complexity:** Average $O(1)$ for `put()`, `get()`, `containsKey()`, and `remove()`.
        
          
        
    - **Key DSA Methods:** `getOrDefault(k, defaultVal)`, `putIfAbsent(k, v)`, `computeIfAbsent(k, key -> new ArrayList<>())`.
        
          
        
    - **DSA Use Cases:** Frequency counting, prefix-sum hash maps (Subarray Sum Equals K), caching/memoization in Dynamic Programming.
        
          
        
- **`TreeMap<K, V>`** (implements `NavigableMap<K, V>`, `SortedMap<K, V>`)
    
      
    - **Underlying DS:** Red-Black Tree.
        
          
        
    - **Complexity:** Guaranteed $O(\log n)$ for `put()`, `get()`, `remove()`.
        
          
        
    - **Key DSA Methods:** `floorKey(k)`, `ceilingKey(k)`, `firstEntry()`, `lastEntry()`.
        
          
        
    - **DSA Use Cases:** Dynamic frequency maps requiring ordered sweeps, timeline/calendar interval bookings.
        
          
        
- **`LinkedHashMap<K, V>`**
    
      
    - **Underlying DS:** Hash table + Doubly linked list.
        
          
        
    - **DSA Use Cases:** Implementing an **LRU Cache** by setting `accessOrder = true` in the constructor and overriding `removeEldestEntry()`.
        

### DSA Selection Matrix

|**Problem Need**|**Preferred Collection**|**Key Alternative**|**Why?**|
|---|---|---|---|
|**Stack / LIFO**|`ArrayDeque<T>`|`LinkedList<T>`|Faster than `Stack` (no synchronization); lower memory than `LinkedList`.|
|**Queue / FIFO / BFS**|`ArrayDeque<T>`|`LinkedList<T>`|Circular array cache locality beats pointer-chasing.|
|**Heap / Greedy / Top-K**|`PriorityQueue<T>`|Custom Heap|Built-in binary heap with custom `Comparator` support.|
|**Lookup / Frequency**|`HashMap<K, V>`|`int[]` (if alphabet)|$O(1)$ average lookups; for ASCII strings, primitive arrays are faster.|
|**Dynamic Order / Range Queries**|`TreeSet<T>` / `TreeMap<K, V>`|Segment Tree|Out-of-the-box logarithmic floor/ceiling/range queries.|
|**Graph Adjacency List**|`ArrayList<ArrayList<Integer>>`|`Map<Integer, List<Integer>>`|Primitive indexing beats hash table lookups when nodes are numbered $0$ to $N-1$.|

### Essential Utility Methods (`java.util.Collections` & `java.util.Arrays`)

Java

```
// 1. Sorting primitives and objects
Arrays.sort(primitiveArray);                          // Dual-Pivot Quicksort: O(n log n)
Arrays.sort(objectArray, (a, b) -> a[0] - b[0]);      // TimSort: O(n log n)
Collections.sort(list, Comparator.naturalOrder());    // TimSort

// 2. Binary Search (Requires sorted input)
int idx = Arrays.binarySearch(sortedArr, target);     // Returns (-(insertion point) - 1) if not found
int idx = Collections.binarySearch(sortedList, target);

// 3. Modifiers & Helpers
Collections.reverse(list);
Collections.swap(list, i, j);
Collections.max(list); / Collections.min(list);
Arrays.fill(dpArray, -1);                             // Fast memoization table initialization
```