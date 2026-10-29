
# Java Sets for DSA & Interviews

## 1. What is a `Set`?

A `Set` is a collection that:

- stores **unique elements**
    
- does **not allow duplicates**
    
- provides operations like:
    
    - add
        
    - remove
        
    - contains/search
        
    - size
        

Example:

```java
Set<Integer> set = new HashSet<>();

set.add(10);
set.add(20);
set.add(10);

System.out.println(set);       // [10, 20]
System.out.println(set.size()); // 2
```

The second `10` is ignored.

The important thing for DSA is:

> **Use a Set when your main concern is membership: "Have I seen this before?"**

---

# 2. The Three Sets You Need to Know

Java has three major implementations you'll encounter:

```java
HashSet
LinkedHashSet
TreeSet
```

Think of them like this:

|Set|Ordering|`add`|`contains`|`remove`|
|---|---|--:|--:|--:|
|`HashSet`|No guaranteed order|O(1)*|O(1)*|O(1)*|
|`LinkedHashSet`|Insertion order|O(1)*|O(1)*|O(1)*|
|`TreeSet`|Sorted order|O(log n)|O(log n)|O(log n)|

`*` = average/amortized expected complexity for `HashSet`-based operations.

For most DSA problems:

> **Start with `HashSet`.**

---

# 3. HashSet — Most Important

```java
Set<Integer> set = new HashSet<>();
```

Internally, `HashSet` is hash-table based.

The key idea:

```text
value
  ↓
hashCode()
  ↓
bucket
```

This allows Java to find an element without scanning the entire collection.

### Basic operations

```java
Set<Integer> set = new HashSet<>();

set.add(5);
set.add(10);
set.add(15);

set.contains(10); // true

set.remove(10);

set.contains(10); // false

set.size();       // 2

set.isEmpty();    // false

set.clear();
```

---

# 4. Why HashSet is so useful in DSA

Suppose you have:

```text
[3, 7, 2, 9, 4, 7, 1]
```

and need to answer:

> Is `7` present?

With an array, you may have to scan:

```text
3 → 7
```

Potentially O(n).

With a `HashSet`:

```java
Set<Integer> set = new HashSet<>();

for (int x : nums) {
    set.add(x);
}

set.contains(7); // O(1) average
```

Building:

O(n)O(n)

Searching each element:

O(1)O(1)

This is why Sets appear constantly in interview problems.

---

# 5. The Most Important Set Pattern

Whenever you see:

- duplicate
    
- unique
    
- already seen
    
- visited
    
- exists
    
- membership
    
- common elements
    
- intersection
    
- difference
    

think:

```java
HashSet
```

---

# 6. HashSet with Strings

```java
Set<String> set = new HashSet<>();

set.add("apple");
set.add("banana");
set.add("apple");

System.out.println(set);
```

Only one `"apple"` exists.

You can also use:

```java
if (set.contains("apple")) {
    System.out.println("Found");
}
```

---

# 7. Iterating over a Set

```java
Set<Integer> set = new HashSet<>();

set.add(10);
set.add(20);
set.add(30);

for (int x : set) {
    System.out.println(x);
}
```

Do **not** assume the order is:

```text
10
20
30
```

`HashSet` does not guarantee iteration order.

If order matters, use `LinkedHashSet` or `TreeSet`.

---

# 8. LinkedHashSet

```java
Set<Integer> set = new LinkedHashSet<>();

set.add(30);
set.add(10);
set.add(20);

System.out.println(set);
```

Output:

```text
[30, 10, 20]
```

It preserves **insertion order**.

Complexity is generally similar to `HashSet`, but it uses additional memory to maintain the linked ordering.

### When useful?

For example:

> Remove duplicates while preserving original order.

```java
int[] nums = {4, 2, 4, 1, 2, 3};

Set<Integer> set = new LinkedHashSet<>();

for (int x : nums) {
    set.add(x);
}

System.out.println(set);
```

Result:

```text
[4, 2, 1, 3]
```

---

# 9. TreeSet

`TreeSet` maintains elements in sorted order.

```java
Set<Integer> set = new TreeSet<>();

set.add(50);
set.add(10);
set.add(30);
set.add(20);

System.out.println(set);
```

Output:

```text
[10, 20, 30, 50]
```

Operations:

```java
set.add(x);       // O(log n)
set.remove(x);    // O(log n)
set.contains(x);  // O(log n)
```

Internally, it is based on a balanced search tree.

---

# 10. TreeSet's Special DSA Methods

This is where `TreeSet` becomes particularly useful.

```java
TreeSet<Integer> set = new TreeSet<>();

set.add(10);
set.add(20);
set.add(30);
set.add(40);
```

### `first()`

```java
set.first(); // 10
```

### `last()`

```java
set.last(); // 40
```

### `higher(x)`

Smallest element **strictly greater than** `x`.

```java
set.higher(20); // 30
```

### `lower(x)`

Largest element **strictly smaller than** `x`.

```java
set.lower(20); // 10
```

### `ceiling(x)`

Smallest element **greater than or equal to** `x`.

```java
set.ceiling(20); // 20
set.ceiling(25); // 30
```

### `floor(x)`

Largest element **less than or equal to** `x`.

```java
set.floor(20); // 20
set.floor(25); // 20
```

These are **very useful in harder DSA problems**.

---

# 11. HashSet vs TreeSet — Interview Question

Suppose interviewer asks:

> Why would you use HashSet instead of TreeSet?

Answer:

**HashSet:**

```text
Average add      O(1)
Average search   O(1)
Average remove   O(1)
No sorting
```

**TreeSet:**

```text
add              O(log n)
search           O(log n)
remove           O(log n)
Elements sorted
Supports floor/ceiling/lower/higher
```

So:

> If I only need fast membership → `HashSet`.

> If I need sorted elements or predecessor/successor queries → `TreeSet`.

---

# 12. Set with Custom Objects

This is a VERY important interview concept.

Suppose:

```java
class Person {
    int age;
    String name;

    Person(int age, String name) {
        this.age = age;
        this.name = name;
    }
}
```

If you do:

```java
Set<Person> set = new HashSet<>();

set.add(new Person(20, "John"));
set.add(new Person(20, "John"));
```

You may get **two objects**.

Why?

Because for `HashSet`, equality is determined using:

```java
equals()
hashCode()
```

You need to override both.

```java
class Person {
    int age;
    String name;

    Person(int age, String name) {
        this.age = age;
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Person)) return false;

        Person other = (Person) obj;

        return age == other.age &&
               name.equals(other.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(age, name);
    }
}
```

Now duplicate logical objects are recognized.

### Interview rule

> If you override `equals()`, you should also override `hashCode()` consistently.

---

# 13. Set of Arrays — Common Trap

This surprises many people:

```java
Set<int[]> set = new HashSet<>();

set.add(new int[]{1, 2});
set.add(new int[]{1, 2});

System.out.println(set.size());
```

Result:

```text
2
```

Why?

Arrays don't override `equals()`/`hashCode()` based on contents. Their default behavior is identity-based.

For content-based equality, use:

```java
Set<List<Integer>> set = new HashSet<>();

set.add(Arrays.asList(1, 2));
set.add(Arrays.asList(1, 2));

System.out.println(set.size()); // 1
```

Or create an appropriate immutable key representation.

---

# 14. Most Important Pattern #1: Detect Duplicates

### LeetCode 217 — Contains Duplicate

Problem:

```text
nums = [1,2,3,1]
```

Return `true`.

### Solution

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {

        Set<Integer> set = new HashSet<>();

        for (int num : nums) {

            if (set.contains(num)) {
                return true;
            }

            set.add(num);
        }

        return false;
    }
}
```

### Complexity

Time:

O(n)O(n)

Space:

O(n)O(n)

### Even cleaner

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<>();

        for (int num : nums) {
            if (!set.add(num)) {
                return true;
            }
        }

        return false;
    }
}
```

This is a great interview trick.

`Set.add()` returns:

```text
true  → element was newly added
false → element already existed
```

So:

```java
if (!set.add(num))
```

means:

> "I've already seen this."

---

# 15. Most Important Pattern #2: Intersection of Arrays

### LeetCode 349 — Intersection of Two Arrays

Example:

```text
nums1 = [1,2,2,1]
nums2 = [2,2]

answer = [2]
```

Use a Set.

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {

        Set<Integer> set = new HashSet<>();

        for (int x : nums1) {
            set.add(x);
        }

        Set<Integer> result = new HashSet<>();

        for (int x : nums2) {
            if (set.contains(x)) {
                result.add(x);
            }
        }

        int[] ans = new int[result.size()];

        int i = 0;

        for (int x : result) {
            ans[i++] = x;
        }

        return ans;
    }
}
```

Complexity:

O(n+m)O(n+m)

Space:

O(n+m)O(n+m)

---

# 16. Most Important Pattern #3: Happy Number

### LeetCode 202 — Happy Number

A number eventually reaches `1`, or gets stuck in a cycle.

The key observation:

> We need to detect whether we've seen a number before.

That's exactly a Set.

```java
class Solution {

    public boolean isHappy(int n) {

        Set<Integer> seen = new HashSet<>();

        while (n != 1) {

            if (!seen.add(n)) {
                return false;
            }

            n = getNext(n);
        }

        return true;
    }

    private int getNext(int n) {

        int sum = 0;

        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }

        return sum;
    }
}
```

### Pattern

```text
while (...)
    if already seen → cycle
    add current state
    move to next state
```

This is an extremely important general DSA pattern.

---

# 17. Most Important Pattern #4: Longest Consecutive Sequence

### LeetCode 128

Given:

```text
[100,4,200,1,3,2]
```

Answer:

```text
4
```

Because:

```text
1,2,3,4
```

### Naive approach

Sort:

```java
Arrays.sort(nums);
```

Complexity:

O(nlog⁡n)O(n\log n)

But we can do better.

### HashSet solution

```java
class Solution {
    public int longestConsecutive(int[] nums) {

        Set<Integer> set = new HashSet<>();

        for (int num : nums) {
            set.add(num);
        }

        int longest = 0;

        for (int num : set) {

            // Start of a sequence
            if (!set.contains(num - 1)) {

                int current = num;
                int length = 1;

                while (set.contains(current + 1)) {
                    current++;
                    length++;
                }

                longest = Math.max(longest, length);
            }
        }

        return longest;
    }
}
```

### Why is this O(n)?

At first glance, the nested `while` looks like O(n²).

But notice:

```java
if (!set.contains(num - 1))
```

We only start walking when `num` is the **beginning of a sequence**.

Each number participates in the sequence traversal only a constant number of times overall.

Therefore:

O(n)O(n)

average time.

Space:

O(n)O(n)

### Interview insight

This is a classic:

> **Put everything in a Set → use O(1) membership → avoid sorting.**

---

# 18. Most Important Pattern #5: Happy/Visited/Cycle Detection

Sets are frequently used when a problem involves a state repeatedly changing.

General structure:

```java
Set<State> seen = new HashSet<>();

while (...) {

    if (seen.contains(state)) {
        // cycle
    }

    seen.add(state);

    state = nextState(state);
}
```

You'll see this in:

- linked list-like state transitions
    
- number transformations
    
- graph traversal
    
- simulations
    
- repeated operations
    

---

# 19. Set in Sliding Window Problems

This is another HUGE interview pattern.

### LeetCode 3 — Longest Substring Without Repeating Characters

Example:

```text
abcabcbb
```

Answer:

```text
3
```

The Set stores characters currently inside the window.

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {

        Set<Character> set = new HashSet<>();

        int left = 0;
        int right = 0;
        int max = 0;

        while (right < s.length()) {

            char c = s.charAt(right);

            while (set.contains(c)) {
                set.remove(s.charAt(left));
                left++;
            }

            set.add(c);

            max = Math.max(max, right - left + 1);

            right++;
        }

        return max;
    }
}
```

### Complexity

Each character enters the Set once and leaves once.

Therefore:

O(n)O(n)

Space:

O(min⁡(n,charset))O(\min(n, \text{charset}))

---

# 20. Set vs Map

This is one of the most important distinctions.

Suppose:

```text
nums = [1,2,3,1]
```

Question:

> Have I seen `1`?

Use:

```java
Set<Integer>
```

But if question is:

> How many times have I seen `1`?

Use:

```java
Map<Integer, Integer>
```

### Set

```java
Set<Integer> seen = new HashSet<>();
```

Represents:

```text
value → exists?
```

### Map

```java
Map<Integer, Integer> freq = new HashMap<>();
```

Represents:

```text
value → information
```

For example:

```text
1 → 2
2 → 1
3 → 1
```

---

# 21. A Great Interview Decision Tree

When you see an array/string problem, ask:

### Question 1

**Do I only need to know whether something exists?**

Use:

```java
HashSet
```

### Question 2

**Do I need frequency/count?**

Use:

```java
HashMap
```

### Question 3

**Do I need sorted unique values?**

Use:

```java
TreeSet
```

### Question 4

**Do I need sorted values + duplicates?**

Use:

```java
TreeMap
```

### Question 5

**Do I need insertion order + uniqueness?**

Use:

```java
LinkedHashSet
```

---

# 22. Set Operations

You can implement mathematical set operations easily.

### Union

```java
Set<Integer> a = new HashSet<>(Arrays.asList(1, 2, 3));
Set<Integer> b = new HashSet<>(Arrays.asList(3, 4, 5));

Set<Integer> union = new HashSet<>(a);
union.addAll(b);

System.out.println(union);
```

Result:

```text
[1, 2, 3, 4, 5]
```

---

### Intersection

```java
Set<Integer> intersection = new HashSet<>(a);
intersection.retainAll(b);
```

Result:

```text
[3]
```

---

### Difference

```java
Set<Integer> difference = new HashSet<>(a);
difference.removeAll(b);
```

Result:

```text
[1, 2]
```

---

### Check subset

```java
Set<Integer> a = new HashSet<>(Arrays.asList(1, 2));
Set<Integer> b = new HashSet<>(Arrays.asList(1, 2, 3));

System.out.println(b.containsAll(a)); // true
```

---

# 23. Important `Set` API to Memorize

For interviews, know these cold:

```java
Set<Integer> set = new HashSet<>();
```

### Add

```java
set.add(x);
```

### Search

```java
set.contains(x);
```

### Remove

```java
set.remove(x);
```

### Size

```java
set.size();
```

### Empty

```java
set.isEmpty();
```

### Clear

```java
set.clear();
```

### Bulk operations

```java
set.addAll(other);
set.removeAll(other);
set.retainAll(other);
set.containsAll(other);
```

### Convert to array

```java
Integer[] arr = set.toArray(new Integer[0]);
```

---

# 24. TreeSet API You Should Know

```java
TreeSet<Integer> set = new TreeSet<>();
```

Know these:

```java
first()
last()

lower(x)
floor(x)

higher(x)
ceiling(x)
```

Example:

```java
TreeSet<Integer> set = new TreeSet<>(
    Arrays.asList(10, 20, 30, 40)
);

set.lower(25);    // 20
set.floor(20);    // 20
set.higher(25);   // 30
set.ceiling(25);  // 30
```

These are extremely useful for:

> Find the closest/smallest/largest value satisfying a condition.

---

# 25. TreeSet Range Queries

You can also do:

```java
set.headSet(30);
```

Elements `< 30`.

```java
set.tailSet(30);
```

Elements `>= 30`.

```java
set.subSet(20, 40);
```

Elements in the range `[20, 40)`.

These can appear in advanced problems.

---

# 26. Custom Comparator with TreeSet

Suppose you want descending order:

```java
TreeSet<Integer> set =
    new TreeSet<>(Collections.reverseOrder());
```

Now:

```text
50
40
30
20
10
```

Or:

```java
TreeSet<String> set =
    new TreeSet<>((a, b) -> b.length() - a.length());
```

**Caution:** comparator logic must be consistent with the equality semantics you intend. A comparator returning `0` means the `TreeSet` treats those two elements as the same set key.

---

# 27. A Very Common TreeSet Interview Trap

Consider:

```java
TreeSet<String> set = new TreeSet<>(
    (a, b) -> a.length() - b.length()
);
```

Then:

```java
set.add("cat");
set.add("dog");
```

What happens?

Only **one** element may be stored.

Because:

```java
"cat".length() == "dog".length()
```

so comparator returns:

```text
0
```

TreeSet considers them equivalent for set purposes.

If you need both:

```java
TreeSet<String> set = new TreeSet<>(
    (a, b) -> {
        int cmp = Integer.compare(a.length(), b.length());
        if (cmp != 0) return cmp;
        return a.compareTo(b);
    }
);
```

---

# 28. LeetCode Problems You Should Practice

If you're preparing for interviews, I'd prioritize these:

### Easy / Foundation

**217 — Contains Duplicate**

Pattern:

```text
HashSet → detect duplicate
```

**349 — Intersection of Two Arrays**

Pattern:

```text
HashSet → membership/intersection
```

**202 — Happy Number**

Pattern:

```text
HashSet → cycle detection
```

**705 — Design HashSet**

Pattern:

```text
Understand HashSet implementation
```

---

### Medium / Very Important

**128 — Longest Consecutive Sequence**

Pattern:

```text
HashSet + sequence start
```

**3 — Longest Substring Without Repeating Characters**

Pattern:

```text
Sliding Window + HashSet
```

**36 — Valid Sudoku**

Pattern:

```text
Sets → row/column/box uniqueness
```

**380 — Insert Delete GetRandom O(1)**

Important because a Set alone isn't enough; you'll learn:

```text
HashMap + ArrayList
```

---

### More Advanced Set/Ordering Problems

**220 — Contains Duplicate III**

Useful for:

```text
TreeSet
floor()
ceiling()
```

**128 — Longest Consecutive Sequence**

One of the most important Set problems.

**1207 — Unique Number of Occurrences**

Pattern:

```text
HashMap + HashSet
```

**2215 — Find the Difference of Two Arrays**

Pattern:

```text
Set difference
```

---

# 29. LeetCode 36 — Valid Sudoku

This is a beautiful example of using multiple Sets.

For every row:

```text
1 2 3 4 ...
```

must contain no duplicates.

Same for columns and 3×3 boxes.

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {

        Set<Character>[] rows = new HashSet[9];
        Set<Character>[] cols = new HashSet[9];
        Set<Character>[] boxes = new HashSet[9];

        for (int i = 0; i < 9; i++) {
            rows[i] = new HashSet<>();
            cols[i] = new HashSet<>();
            boxes[i] = new HashSet<>();
        }

        for (int r = 0; r < 9; r++) {

            for (int c = 0; c < 9; c++) {

                char val = board[r][c];

                if (val == '.') {
                    continue;
                }

                int box = (r / 3) * 3 + (c / 3);

                if (!rows[r].add(val)) {
                    return false;
                }

                if (!cols[c].add(val)) {
                    return false;
                }

                if (!boxes[box].add(val)) {
                    return false;
                }
            }
        }

        return true;
    }
}
```

The key line:

```java
if (!rows[r].add(val))
```

means:

> If adding it failed, we've already seen it.

This pattern is worth memorizing.

---

# 30. Set + String Problems

For strings, Sets are often used to track:

```text
characters seen
words seen
substrings/states seen
```

Example:

```java
Set<Character> seen = new HashSet<>();

for (char c : s) {
    if (!seen.add(c)) {
        // duplicate
    }
}
```

You can also initialize:

```java
Set<Character> set = new HashSet<>();

for (char c : s.toCharArray()) {
    set.add(c);
}
```

---

# 31. Primitive Types and Sets

Java Collections cannot directly store primitives.

You cannot do:

```java
Set<int> set; // ❌
```

Use:

```java
Set<Integer> set; // ✅
```

Java automatically boxes:

```java
int x = 5;

set.add(x);
```

into an `Integer`.

For most LeetCode problems, this is perfectly fine.

---

# 32. HashSet and `null`

A `HashSet` can contain one `null`:

```java
Set<Integer> set = new HashSet<>();

set.add(null);
set.add(null);

System.out.println(set.size()); // 1
```

`TreeSet` generally does **not** allow `null` with natural ordering.

This isn't usually important for LeetCode, but it can appear in Java interviews.

---

# 33. HashSet Complexity — The Interview Answer

For a `HashSet`:

```text
add       average O(1)
contains  average O(1)
remove    average O(1)
```

But don't blindly say:

> HashSet is always O(1).

The more accurate interview answer is:

> HashSet provides expected/average constant-time basic operations under good hash distribution; pathological collision behavior can degrade, while Java's implementation has protections against severe hash collisions for suitable key types.

For normal DSA problems:

```text
Expected: O(1)
```

is the answer you typically use.

---

# 34. How HashSet Works Internally

Conceptually:

```text
HashSet
   ↓
HashMap
   ↓
hash table
```

When you execute:

```java
set.add(42);
```

Java hashes the object:

```text
42
 ↓
hashCode()
 ↓
bucket
```

Then it stores the object in that bucket.

When you execute:

```java
set.contains(42);
```

Java hashes `42` again and looks in the relevant bucket.

That's why lookup can be approximately constant time.

---

# 35. Why `hashCode()` and `equals()` Both Matter

Suppose:

```java
Set<Person> set = new HashSet<>();
```

Java roughly needs to answer:

> Which bucket should I look in?

That's `hashCode()`.

Then:

> Is this actually the same object/value?

That's where `equals()` matters.

The contract is:

```text
a.equals(b) == true
        ↓
a.hashCode() must equal b.hashCode()
```

The reverse isn't necessarily true:

```text
same hashCode
≠
same object
```

That's because hash collisions can happen.

---

# 36. The Most Useful DSA Mental Model

Don't think:

> "This is a Set problem."

Think:

> **"Can I trade O(n) searching for O(1) membership by storing things in a hash table?"**

Example:

```text
Array:
[10, 20, 30, 40, 50]
```

Searching repeatedly:

```text
O(n) per search
```

Store in Set:

```java
Set<Integer> set = new HashSet<>(...);
```

Now:

```java
set.contains(x)
```

is expected O(1).

This can turn:

O(n2)O(n^2)

into:

O(n)O(n)

which is one of the most important optimization patterns in DSA.

---

# 37. Set Pattern Cheat Sheet

|Problem says...|Think|
|---|---|
|Detect duplicate|`HashSet`|
|Have I seen this?|`HashSet`|
|Unique elements|`HashSet`|
|Intersection|`HashSet`|
|Difference|`HashSet`|
|Visited states|`HashSet`|
|Cycle detection|`HashSet`|
|Longest consecutive sequence|`HashSet`|
|Sliding window unique chars|`HashSet`|
|Sorted unique values|`TreeSet`|
|Next greater available value|`TreeSet.higher()`|
|Next smaller available value|`TreeSet.lower()`|
|Closest >= x|`TreeSet.ceiling()`|
|Closest <= x|`TreeSet.floor()`|
|Preserve insertion order|`LinkedHashSet`|
|Count occurrences|`HashMap`, not Set|

---

# 38. The 5 Set Patterns I'd Master for Interviews

If you're short on time, focus heavily on these:

### Pattern 1 — Seen Set

```java
Set<Integer> seen = new HashSet<>();

for (int x : nums) {
    if (!seen.add(x)) {
        return true;
    }
}
```

Used for:

**217, 202, many duplicate/cycle problems**

---

### Pattern 2 — Membership Lookup

```java
Set<Integer> set = new HashSet<>(...);

if (set.contains(target)) {
    ...
}
```

Used for:

**349, 128, array lookup problems**

---

### Pattern 3 — Set + Sliding Window

```java
Set<Character> set = new HashSet<>();

while (...) {

    while (set.contains(c)) {
        set.remove(...);
    }

    set.add(c);
}
```

Used for:

**LeetCode 3 and related substring/window problems.**

---

### Pattern 4 — Set + State/Cycle Detection

```java
Set<State> seen = new HashSet<>();

while (...) {

    if (!seen.add(state)) {
        return ...;
    }

    state = next(state);
}
```

Used for:

**202 and many state-space problems.**

---

### Pattern 5 — TreeSet Neighbor Search

```java
TreeSet<Integer> set = new TreeSet<>();

Integer lower = set.floor(x);
Integer upper = set.ceiling(x);
```

Used when you need:

```text
predecessor
successor
closest value
ordered membership
range queries
```

Especially useful for:

**LeetCode 220 and more advanced problems.**

---

## Final interview cheat sheet

```java
// General
Set<Integer> set = new HashSet<>();

// Preserve insertion order
Set<Integer> set = new LinkedHashSet<>();

// Sorted
TreeSet<Integer> set = new TreeSet<>();

// Add
set.add(x);

// Check
set.contains(x);

// Remove
set.remove(x);

// Size
set.size();

// Union
a.addAll(b);

// Intersection
a.retainAll(b);

// Difference
a.removeAll(b);
```

And remember:

```text
HashSet       → fast membership
LinkedHashSet → fast membership + insertion order
TreeSet       → sorted membership + floor/ceiling
HashMap       → key → value / frequency
```

**Best LeetCode progression:** `217 → 349 → 202 → 128 → 3 → 36 → 1207 → 2215 → 705 → 220 → 380`.

If you're preparing seriously for Java interviews, the next step after this is **HashMap + frequency counting + Set/Map patterns**, because a large fraction of array/string LeetCode problems reduce to choosing correctly between `HashSet`, `HashMap`, sorting, and two pointers.