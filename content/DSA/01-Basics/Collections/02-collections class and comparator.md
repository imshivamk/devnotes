The `java.util.Collections` class is a utility toolkit consisting exclusively of static methods that operate on or return collections (like `List`, `Set`, and `Map`).

  

To control how these methods organize custom data, Java relies on two fundamental comparison interfaces: **`Comparable`** and **`Comparator`**.

  

### Key Methods in the `Collections` Utility Class

#### 1. Sorting and Reordering

|**Method**|**Description**|**Time Complexity**|
|---|---|---|
|`Collections.sort(list)`|Sorts a list using the elements' natural ordering (`Comparable`).|$O(n \log n)$ (TimSort)|
|`Collections.sort(list, comparator)`|Sorts a list using a specified custom `Comparator`.|$O(n \log n)$ (TimSort)|
|`Collections.reverse(list)`|Reverses the elements of the specified list in-place.|$O(n)$|
|`Collections.shuffle(list)`|Randomly permutes the list using a default or custom source of randomness (Fisher-Yates).|$O(n)$|
|`Collections.swap(list, i, j)`|Swaps elements at index `i` and `j`.|$O(1)$|
|`Collections.rotate(list, distance)`|Rotates elements by a given distance. Positive shifts right, negative shifts left.|$O(n)$|

Java

```
List<Integer> nums = new ArrayList<>(List.of(4, 2, 8, 1, 5));

Collections.sort(nums);                           // [1, 2, 4, 5, 8]
Collections.sort(nums, Collections.reverseOrder());// [8, 5, 4, 2, 1]
Collections.swap(nums, 0, nums.size() - 1);       // Swaps first and last
Collections.shuffle(nums);                        // Randomized order
```

#### 2. Searching and Extremes

- **`Collections.binarySearch(list, key)`**
    
      
    - Searches a sorted list in $O(\log n)$ time.
        
          
        
    - **Return value:** If present, returns the index ($\ge 0$). If absent, returns `(-(insertion point) - 1)`.
        
          
        
    - _Precondition:_ The list must already be sorted in ascending order according to the key's natural ordering or supplied comparator.
        
          
        
- **`Collections.min(coll)` / `Collections.max(coll)`**
    
      
    - Finds minimum/maximum element in $O(n)$ time based on natural ordering or a provided `Comparator`.
        
          
        
- **`Collections.frequency(coll, obj)`**
    
      
    - Counts occurrences of `obj` in the collection using `.equals()` in $O(n)$ time.
        
          
        
- **`Collections.disjoint(c1, c2)`**
    
      
    - Returns `true` if two collections share **no common elements**.
        
          
        

Java

```
List<Integer> sorted = List.of(10, 20, 30, 40, 50);

int idx = Collections.binarySearch(sorted, 30); // Returns 2
int missing = Collections.binarySearch(sorted, 25); // Returns -3 (insertion point is index 2)

int maxVal = Collections.max(sorted); // 50
int count = Collections.frequency(List.of(1, 2, 2, 3, 2), 2); // 3
```

#### 3. Thread-Safety Wrappers (`synchronizedXxx`)

Wrap non-thread-safe collections with synchronized wrappers. Every method is guarded by a single mutex lock.

  

Java

```
List<String> syncList = Collections.synchronizedList(new ArrayList<>());
Set<Integer> syncSet  = Collections.synchronizedSet(new HashSet<>());
Map<String, Integer> syncMap = Collections.synchronizedMap(new HashMap<>());
```

> **Manual Synchronization Rule:** Iteration over a synchronized collection must still be manually synchronized on the wrapper object to prevent concurrent modifications during traversal:
> 
>   
> 
> Java
> 
> ```
> synchronized (syncList) {
>     for (String s : syncList) {
>         System.out.println(s);
>     }
> }
> ```

#### 4. Unmodifiable & Immutable Views (`unmodifiableXxx`)

Creates a read-only wrapper around an existing collection. Any mutating method (`add`, `remove`, `set`) throws `UnsupportedOperationException`.

  

Java

```
List<String> mutable = new ArrayList<>(List.of("A", "B"));
List<String> readOnly = Collections.unmodifiableList(mutable);

// readOnly.add("C"); // Throws UnsupportedOperationException
```

_Note:_ `unmodifiableXxx` is a view—if the underlying `mutable` list changes, the `readOnly` view reflects those changes. For fully immutable collections in Java 9+, use `List.of()`, `Set.of()`, or `Map.of()`.

  

#### 5. Specialized Utility Collections

- **`Collections.emptyList()` / `emptySet()` / `emptyMap()`**: Returns type-safe, immutable empty collections without allocating new heap memory on each call. Ideal for default/fallback return values.
    
      
    
- **`Collections.singleton(obj)` / `singletonList(obj)` / `singletonMap(k, v)`**: Returns an immutable single-element collection with minimal memory footprint.
    
      
    
- **`Collections.nCopies(n, obj)`**: Returns an immutable list of $n$ references to `obj` in $O(1)$ space (allocates no backing array).
    

Java

```
// Fast way to initialize a dynamic list with 10 zeros
List<Integer> zeros = new ArrayList<>(Collections.nCopies(10, 0));
```


### Understanding Ordering: `Comparable` vs. `Comparator`

When sorting or managing ordered collections (`TreeSet`, `TreeMap`, `PriorityQueue`), Java needs to know how to compare two elements.

  

```
                    ┌───────────────────────────────┐
                    │      How to compare data?     │
                    └───────────────┬───────────────┘
                                    │
            ┌───────────────────────┴───────────────────────┐
            ▼                                               ▼
   Comparable<T> (Internal)                        Comparator<T> (External)
   • "I define my own default order"               • "A third party defines custom order"
   • Method: compareTo(T other)                    • Method: compare(T o1, T o2)
   • Single, natural sort strategy                 • Multiple, swappable sort strategies
```

#### 1. `Comparable<T>` (Natural Ordering)

Implemented directly inside the class whose objects you want to sort.

  

- **Method Signature:** `int compareTo(T o)`
    
      
    
- **Return Contract:**
    
      
    - **Negative integer ($< 0$):** `this` is smaller than `o` (comes before).
        
          
        
    - **Zero ($0$):** `this` is equal to `o`.
        
          
        
    - **Positive integer ($> 0$):** `this` is greater than `o` (comes after).
        
          
        

Java

```
public class Student implements Comparable<Student> {
    int rollNo;
    String name;

    public Student(int rollNo, String name) {
        this.rollNo = rollNo;
        this.name = name;
    }

    @Override
    public int compareTo(Student other) {
        // Natural ordering by roll number ascending
        return Integer.compare(this.rollNo, other.rollNo);
    }
}
```

#### 2. `Comparator<T>` (Custom / External Ordering)

A functional interface defined outside the target class. It allows you to sort by multiple criteria (e.g., sort by name, sort by score descending) without modifying the original class.

  

- **Method Signature:** `int compare(T o1, T o2)`
    
      
    
- **Return Contract:**
    
      
    - **$< 0$**: `o1` comes before `o2`.
        
          
        
    - **$0$**: `o1` and `o2` are equal.
        
          
        
    - **$> 0$**: `o1` comes after `o2`.
        
          
        

**Modern Comparator Patterns (Java 8+):**

  

Java

```
// 1. Lambda syntax (by name ascending)
Comparator<Student> byName = (s1, s2) -> s1.name.compareTo(s2.name);

// 2. Method reference with Comparator factory methods
Comparator<Student> byRoll = Comparator.comparingInt(s -> s.rollNo);

// 3. Multi-level chaining (Sort by Name, then by Roll No descending)
Comparator<Student> multiSort = Comparator.comparing((Student s) -> s.name)
                                          .thenComparing(s -> s.rollNo, Comparator.reverseOrder());
```

> **Subtraction Gotcha:** Avoid `(a, b) -> a - b` for integer comparisons. If `a` is a large positive number and `b` is a large negative number, `a - b` can overflow/underflow and produce incorrect comparison results. Always use `Integer.compare(a, b)`.
> 
>   

