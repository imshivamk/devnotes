> learn how hashing works in java : [[hashing in java]]
# Java HashMap for DSA & Interviews

## 1. What is a HashMap?

A `HashMap<K, V>` stores **key → value** pairs.

```java
HashMap<String, Integer> map = new HashMap<>();

map.put("Alice", 25);
map.put("Bob", 30);

System.out.println(map.get("Alice")); // 25
```

Think of it like:

```text
Key       Value
--------------
"Alice"    25
"Bob"      30
```

### Important properties

|Property|HashMap|
|---|---|
|Stores|Key-value pairs|
|Duplicate keys|❌|
|Duplicate values|✅|
|Ordering|❌ No guaranteed order|
|`get()`|Average O(1)|
|`put()`|Average O(1)|
|`remove()`|Average O(1)|
|Allows null key|✅ One|
|Allows null values|✅|

---

# 2. The most important idea: Hashing

Suppose:

```java
map.put("apple", 10);
```

Java needs to figure out **where to store `"apple"`**.

Conceptually:

```text
"apple"
   ↓
hashCode()
   ↓
hash
   ↓
bucket/index
   ↓
store key-value pair
```

When you do:

```java
map.get("apple");
```

Java hashes `"apple"` again and goes directly to the relevant bucket.

That's why lookup is approximately:

```text
O(1)
```

instead of searching every element:

```text
O(n)
```

---

# 3. HashMap vs HashSet

This is extremely important for DSA.

### HashMap

Stores:

```text
key → value
```

```java
HashMap<Integer, Integer> map = new HashMap<>();

map.put(5, 100);
```

### HashSet

Stores only unique values:

```java
HashSet<Integer> set = new HashSet<>();

set.add(5);
set.add(10);
```

Conceptually:

```text
HashMap:
5 → 100
10 → 200

HashSet:
5
10
```

### Rule of thumb

Ask yourself:

> **Do I need to associate information with something?**

Use `HashMap`.

> **Do I only need to know whether something exists?**

Use `HashSet`.

---

# 4. HashMap methods you absolutely need

## `put()`

```java
map.put("apple", 10);
```

If the key already exists, the value is replaced.

```java
map.put("apple", 10);
map.put("apple", 20);

System.out.println(map.get("apple")); // 20
```

---

## `get()`

```java
int value = map.get("apple");
```

If the key doesn't exist:

```java
map.get("banana"); // null
```

This can cause problems with primitive unboxing:

```java
int x = map.get("banana"); // NullPointerException
```

---

# 5. `getOrDefault()` — VERY important for DSA

You'll use this constantly.

```java
map.getOrDefault(key, 0)
```

Means:

> Give me the value associated with `key`; if it doesn't exist, give me `0`.

Example:

```java
HashMap<Integer, Integer> map = new HashMap<>();

int x = map.getOrDefault(5, 0);

System.out.println(x); // 0
```

Then:

```java
map.put(5, map.getOrDefault(5, 0) + 1);
```

This means:

```text
frequency[5]++
```

It's one of the most useful HashMap patterns in LeetCode.

---

# 6. `containsKey()`

```java
if (map.containsKey(5)) {
    System.out.println("Found");
}
```

Returns:

```text
true / false
```

---

# 7. `remove()`

```java
map.remove(5);
```

---

# 8. Iterating through a HashMap

### Keys

```java
for (Integer key : map.keySet()) {
    System.out.println(key);
}
```

### Values

```java
for (Integer value : map.values()) {
    System.out.println(value);
}
```

### Key + Value

Most useful:

```java
for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey());
    System.out.println(entry.getValue());
}
```

Or:

```java
for (var entry : map.entrySet()) {
    System.out.println(entry.getKey() + " " + entry.getValue());
}
```

---

# 9. The #1 HashMap pattern: Frequency Counting

This is probably the most important HashMap pattern for DSA.

Suppose:

```text
nums = [1, 2, 2, 3, 1, 1]
```

We want:

```text
1 → 3
2 → 2
3 → 1
```

### Code

```java
HashMap<Integer, Integer> freq = new HashMap<>();

for (int num : nums) {
    freq.put(num, freq.getOrDefault(num, 0) + 1);
}
```

Result:

```text
{1=3, 2=2, 3=1}
```

### Mental model

Whenever you see:

> count occurrences

Think:

```java
HashMap<T, Integer>
```

---

# 10. Frequency counting strings

Example:

```text
"banana"
```

Want:

```text
b → 1
a → 3
n → 2
```

```java
HashMap<Character, Integer> freq = new HashMap<>();

for (char c : s.toCharArray()) {
    freq.put(c, freq.getOrDefault(c, 0) + 1);
}
```

---

# 11. Pattern: Frequency comparison

Suppose:

```text
s = "anagram"
t = "nagaram"
```

We can count characters.

```java
HashMap<Character, Integer> map = new HashMap<>();

for (char c : s.toCharArray()) {
    map.put(c, map.getOrDefault(c, 0) + 1);
}

for (char c : t.toCharArray()) {
    map.put(c, map.getOrDefault(c, 0) - 1);
}
```

Then check whether every frequency is zero.

This pattern is useful for:

- Anagrams
    
- Permutations
    
- Character matching
    
- Frequency-based problems
    

---

# 12. LeetCode #1 — Two Sum ⭐⭐⭐⭐⭐

This is probably the **most important HashMap problem** to understand.

Problem:

```text
nums = [2, 7, 11, 15]
target = 9
```

Need:

```text
[0, 1]
```

because:

```text
2 + 7 = 9
```

---

## Brute force

```java
for (int i = 0; i < nums.length; i++) {
    for (int j = i + 1; j < nums.length; j++) {
        if (nums[i] + nums[j] == target) {
            return new int[]{i, j};
        }
    }
}
```

Complexity:

```text
O(n²)
```

---

## HashMap solution

The key idea:

For current number:

```text
num
```

we need:

```text
target - num
```

Example:

```text
target = 9

num = 2
needed = 9 - 2 = 7
```

Store numbers we've already seen.

```java
public int[] twoSum(int[] nums, int target) {

    HashMap<Integer, Integer> map = new HashMap<>();

    for (int i = 0; i < nums.length; i++) {

        int needed = target - nums[i];

        if (map.containsKey(needed)) {
            return new int[]{map.get(needed), i};
        }

        map.put(nums[i], i);
    }

    return new int[]{};
}
```

### Dry run

```text
nums = [2, 7, 11, 15]
target = 9
```

Initially:

```text
map = {}
```

`2`:

```text
needed = 7

7 not found

map = {2 → 0}
```

`7`:

```text
needed = 2

2 found!

return [0, 1]
```

### Complexity

```text
Time:  O(n)
Space: O(n)
```

---

# 13. The general "Complement" Pattern

Two Sum teaches a broader pattern:

> **I have X. I need Y. Can I quickly determine whether Y already exists?**

Formula:

```java
needed = target - current
```

Then:

```java
if (map.containsKey(needed))
```

You'll see this idea in many problems.

---

# 14. Pattern: Seen elements

Suppose:

```text
nums = [1, 2, 3, 1]
```

Question:

> Does the array contain duplicates?

Use `HashSet`.

```java
public boolean containsDuplicate(int[] nums) {

    HashSet<Integer> set = new HashSet<>();

    for (int num : nums) {

        if (set.contains(num)) {
            return true;
        }

        set.add(num);
    }

    return false;
}
```

Complexity:

```text
O(n) time
O(n) space
```

### Pattern

Whenever the question is:

> Have I seen this before?

Think:

```java
HashSet
```

---

# 15. LeetCode #49 — Group Anagrams ⭐⭐⭐⭐⭐

One of the most important HashMap problems.

Input:

```text
["eat", "tea", "tan", "ate", "nat", "bat"]
```

Output:

```text
[
    ["eat","tea","ate"],
    ["tan","nat"],
    ["bat"]
]
```

The key insight:

Anagrams have the same character frequencies.

For example:

```text
eat
```

frequency:

```text
a → 1
e → 1
t → 1
```

`tea` has the same.

A simpler approach is to sort each string.

```java
HashMap<String, List<String>> map = new HashMap<>();

for (String s : strs) {

    char[] chars = s.toCharArray();

    Arrays.sort(chars);

    String key = new String(chars);

    map.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
}
```

Then:

```java
return new ArrayList<>(map.values());
```

### Important pattern

```text
HashMap<canonical-form, list>
```

Here:

```text
canonical-form = sorted string
```

This pattern appears frequently.

---

# 16. `computeIfAbsent()`

This is useful when your HashMap contains collections.

Instead of:

```java
if (!map.containsKey(key)) {
    map.put(key, new ArrayList<>());
}

map.get(key).add(value);
```

you can write:

```java
map.computeIfAbsent(key, k -> new ArrayList<>())
   .add(value);
```

Think:

```text
"If key doesn't exist, create its value."
```

Very useful for:

```java
HashMap<Integer, List<Integer>>
```

and

```java
HashMap<String, List<String>>
```

---

# 17. Pattern: HashMap + Prefix Sum ⭐⭐⭐⭐⭐

This is a **very important interview pattern**.

Consider:

```text
nums = [1, 2, 3]
```

Prefix sums:

```text
index:       0   1   2
nums:        1   2   3
prefix:      1   3   6
```

Suppose we want to find a subarray whose sum is `k`.

If:

```text
prefix[j] - prefix[i] = k
```

then:

```text
prefix[i] = prefix[j] - k
```

So while traversing, we ask:

> Have I previously seen `currentPrefix - k`?

That's exactly what a HashMap can answer in O(1).

---

# 18. LeetCode #560 — Subarray Sum Equals K ⭐⭐⭐⭐⭐

```java
public int subarraySum(int[] nums, int k) {

    HashMap<Integer, Integer> map = new HashMap<>();

    map.put(0, 1);

    int prefixSum = 0;
    int count = 0;

    for (int num : nums) {

        prefixSum += num;

        int needed = prefixSum - k;

        if (map.containsKey(needed)) {
            count += map.get(needed);
        }

        map.put(
            prefixSum,
            map.getOrDefault(prefixSum, 0) + 1
        );
    }

    return count;
}
```

The magical line:

```java
map.put(0, 1);
```

means:

> Before processing anything, we've seen prefix sum `0` once.

This handles subarrays starting at index `0`.

---

# 19. Another major pattern: Index tracking

Sometimes you don't just want to know:

> Does this value exist?

You want:

> **Where did I see it?**

Use:

```java
HashMap<Integer, Integer>
```

where:

```text
value → index
```

Example:

```java
map.put(nums[i], i);
```

Two Sum uses exactly this.

---

# 20. Pattern: Last occurrence

Very common with strings.

Example:

```text
s = "abcabcbb"
```

For longest substring without repeating characters, you can store:

```text
character → latest index
```

Example:

```text
a → 3
b → 4
c → 5
```

This lets you jump the left pointer forward.

---

# 21. LeetCode #3 — Longest Substring Without Repeating Characters ⭐⭐⭐⭐⭐

```java
public int lengthOfLongestSubstring(String s) {

    HashMap<Character, Integer> map = new HashMap<>();

    int left = 0;
    int maxLength = 0;

    for (int right = 0; right < s.length(); right++) {

        char c = s.charAt(right);

        if (map.containsKey(c)) {
            left = Math.max(left, map.get(c) + 1);
        }

        map.put(c, right);

        maxLength = Math.max(maxLength, right - left + 1);
    }

    return maxLength;
}
```

This combines:

```text
HashMap
+
Sliding Window
```

This combination is **extremely common**.

---

# 22. Pattern: HashMap + Sliding Window

When you see:

> longest/shortest substring/subarray satisfying some frequency condition

Think:

```text
Sliding Window
+
HashMap
```

Examples:

- Longest substring without repeating characters
    
- Minimum window substring
    
- Permutation in string
    
- Find all anagrams in a string
    
- Longest substring with at most K distinct characters
    

---

# 23. LeetCode #76 — Minimum Window Substring

This is a harder version.

Typical structure:

```java
HashMap<Character, Integer> need = new HashMap<>();
HashMap<Character, Integer> window = new HashMap<>();
```

Then:

```text
expand right
   ↓
add character
   ↓
window satisfies requirement?
   ↓
shrink left
   ↓
record minimum
```

This is a pattern worth mastering after you understand basic sliding windows.

---

# 24. Pattern: Mapping one thing to another

HashMaps aren't only for numbers.

Example:

```text
A → B
B → A
```

or:

```text
employee → manager
student → grade
character → frequency
number → index
word → count
```

Generic idea:

```java
HashMap<K, V>
```

---

# 25. LeetCode #205 — Isomorphic Strings

Example:

```text
egg
add
```

Mapping:

```text
e → a
g → d
```

But you must also ensure the mapping works in reverse.

```java
public boolean isIsomorphic(String s, String t) {

    HashMap<Character, Character> map1 = new HashMap<>();
    HashMap<Character, Character> map2 = new HashMap<>();

    for (int i = 0; i < s.length(); i++) {

        char a = s.charAt(i);
        char b = t.charAt(i);

        if (map1.containsKey(a) && map1.get(a) != b) {
            return false;
        }

        if (map2.containsKey(b) && map2.get(b) != a) {
            return false;
        }

        map1.put(a, b);
        map2.put(b, a);
    }

    return true;
}
```

Important concept:

```text
one-to-one mapping
```

---

# 26. Pattern: HashMap + Sorting

Sometimes you can create a unique "signature" for an object.

Example:

```text
eat → aet
tea → aet
ate → aet
```

Then:

```java
HashMap<String, List<String>>
```

This is the core of Group Anagrams.

Other signatures can be:

```text
frequency array
sorted string
difference pattern
character count
```

---

# 27. HashMap with arrays

You can use arrays as values.

```java
HashMap<Integer, int[]> map = new HashMap<>();
```

Or:

```java
HashMap<String, int[]> map = new HashMap<>();
```

But be careful with arrays as **keys**.

This:

```java
HashMap<int[], Integer>
```

usually isn't what you want because arrays use identity-based `equals()`/`hashCode()`.

Instead, use:

```java
Arrays.toString(arr)
```

or a custom object.

---

# 28. HashMap with custom objects

This is important for interviews.

Suppose:

```java
class Person {
    String name;
    int age;
}
```

If you want:

```java
HashMap<Person, Integer>
```

you need to understand:

```java
equals()
hashCode()
```

### Rule

If two objects are logically equal:

```java
a.equals(b) == true
```

then they **must have the same `hashCode()`**.

Example:

```java
class Person {

    String name;
    int age;

    @Override
    public boolean equals(Object obj) {

        if (this == obj) return true;

        if (!(obj instanceof Person)) return false;

        Person p = (Person) obj;

        return age == p.age &&
               name.equals(p.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

This is more important for **Java interviews** than for basic LeetCode.

---

# 29. HashMap collisions

Interviewers sometimes ask:

> What happens if two keys produce the same bucket?

That's called a **collision**.

Conceptually:

```text
Key A ──┐
        ├── Bucket 5
Key B ──┘
```

Java handles collisions internally.

Modern Java `HashMap` uses bucket structures that can involve linked nodes and, under sufficiently large collision chains, tree structures.

You don't normally implement this yourself in DSA.

Know the concept:

```text
hash collision
      ↓
same bucket
      ↓
HashMap resolves collision internally
```

---

# 30. HashMap vs TreeMap vs LinkedHashMap

Know this table for interviews.

||HashMap|LinkedHashMap|TreeMap|
|---|---|---|---|
|Ordering|None guaranteed|Insertion/access order|Sorted|
|`get`|O(1) avg|O(1) avg|O(log n)|
|`put`|O(1) avg|O(1) avg|O(log n)|
|Null key|Yes|Yes|Generally no|
|Use when|Fast lookup|Need insertion order|Need sorted keys|

For most LeetCode problems:

```text
HashMap
```

is your default.

---

# 31. The HashMap patterns you should memorize

Don't memorize 50 problems.

Memorize these **patterns**.

### Pattern 1 — Frequency

```java
map.put(x, map.getOrDefault(x, 0) + 1);
```

Used for:

```text
frequency
duplicates
anagrams
counts
majority elements
```

---

### Pattern 2 — Seen

```java
HashSet<Integer> set = new HashSet<>();

if (set.contains(x)) {
    // already seen
}

set.add(x);
```

Used for:

```text
duplicates
visited elements
existence
unique values
```

---

### Pattern 3 — Value → Index

```java
map.put(value, index);
```

Used for:

```text
Two Sum
last occurrence
index lookup
```

---

### Pattern 4 — Complement

```java
int needed = target - current;

if (map.containsKey(needed)) {
    ...
}
```

Used for:

```text
Two Sum
pair problems
difference problems
```

---

### Pattern 5 — Prefix Sum + HashMap

```java
prefix += num;

if (map.containsKey(prefix - k)) {
    answer += map.get(prefix - k);
}

map.put(prefix, map.getOrDefault(prefix, 0) + 1);
```

Used for:

```text
subarray sum
subarray count
zero-sum subarrays
```

---

### Pattern 6 — Sliding Window + HashMap

```java
map.put(c, map.getOrDefault(c, 0) + 1);
```

while maintaining:

```text
left ←→ right
```

Used for:

```text
substring
frequency constraints
anagrams
longest/shortest windows
```

---

### Pattern 7 — Group by key

```java
map.computeIfAbsent(key, k -> new ArrayList<>())
   .add(value);
```

Used for:

```text
Group Anagrams
grouping
categorization
graph adjacency lists
```

---

# 32. Most important HashMap LeetCode problems

If you're working through **Striver A2Z/DSA**, I'd prioritize these.

### 🟢 Easy

|Problem|Main pattern|
|---|---|
|**1. Two Sum** ⭐⭐⭐⭐⭐|Complement + HashMap|
|**217. Contains Duplicate** ⭐⭐⭐⭐|HashSet|
|**242. Valid Anagram** ⭐⭐⭐⭐⭐|Frequency|
|**387. First Unique Character** ⭐⭐⭐⭐|Frequency + scan|
|**349. Intersection of Two Arrays** ⭐⭐⭐|HashSet|
|**205. Isomorphic Strings** ⭐⭐⭐⭐|Mapping|

### 🟡 Medium

|Problem|Main pattern|
|---|---|
|**49. Group Anagrams** ⭐⭐⭐⭐⭐|HashMap + signature|
|**3. Longest Substring Without Repeating Characters** ⭐⭐⭐⭐⭐|Sliding Window + HashMap|
|**347. Top K Frequent Elements** ⭐⭐⭐⭐⭐|Frequency + Heap/Bucket|
|**560. Subarray Sum Equals K** ⭐⭐⭐⭐⭐|Prefix Sum + HashMap|
|**128. Longest Consecutive Sequence** ⭐⭐⭐⭐⭐|HashSet|
|**454. 4Sum II** ⭐⭐⭐⭐|Frequency/complement|
|**380. Insert Delete GetRandom O(1)** ⭐⭐⭐⭐|HashMap + ArrayList|

### 🔴 Hard

|Problem|Main pattern|
|---|---|
|**76. Minimum Window Substring** ⭐⭐⭐⭐⭐|Sliding Window + Frequency|
|**460. LFU Cache** ⭐⭐⭐⭐⭐|HashMap + Linked structures|
|**146. LRU Cache** ⭐⭐⭐⭐⭐|HashMap + Doubly Linked List|

---

# 33. A very useful decision tree

When you encounter an array/string problem, ask:

```text
                    Problem
                       │
                       ▼
             Need fast existence?
                  /         \
                YES          NO
                 │            │
             HashSet          ...
                 │
                 ▼
          Need associated data?
                 │
                YES
                 │
              HashMap
```

Or:

```text
"How many times?"
        ↓
HashMap frequency


"Have I seen this?"
        ↓
HashSet


"Where did I see this?"
        ↓
HashMap<T, Integer>


"I need target - current"
        ↓
HashMap complement


"Subarray sum = K?"
        ↓
Prefix Sum + HashMap


"Substring with constraints?"
        ↓
Sliding Window + HashMap


"Group things by a property?"
        ↓
HashMap<key, List<...>>
```

---

# 34. The most important mental shift

Don't think:

> "This problem is a HashMap problem."

Instead ask:

> **What information would I like to remember while traversing the input?**

For example:

### Two Sum

I want to remember:

```text
number → index
```

Therefore:

```java
HashMap<Integer, Integer>
```

### Frequency

I want:

```text
number → count
```

Therefore:

```java
HashMap<Integer, Integer>
```

### Group Anagrams

I want:

```text
signature → list of strings
```

Therefore:

```java
HashMap<String, List<String>>
```

### Subarray Sum

I want:

```text
prefix sum → number of times seen
```

Therefore:

```java
HashMap<Integer, Integer>
```

### Isomorphic Strings

I want:

```text
character → mapped character
```

Therefore:

```java
HashMap<Character, Character>
```

That's the real skill.

---

# 35. HashMap syntax cheat sheet

Keep this handy while doing DSA:

```java
// Create
HashMap<Integer, Integer> map = new HashMap<>();

// Insert
map.put(5, 100);

// Get
map.get(5);

// Get with default
map.getOrDefault(5, 0);

// Check key
map.containsKey(5);

// Check value
map.containsValue(100);

// Remove
map.remove(5);

// Size
map.size();

// Empty
map.isEmpty();

// Clear
map.clear();

// Keys
for (int key : map.keySet()) {
}

// Values
for (int value : map.values()) {
}

// Key + value
for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
    int key = entry.getKey();
    int value = entry.getValue();
}
```

### Most-used lines in LeetCode

You'll probably write these **constantly**:

```java
map.put(x, map.getOrDefault(x, 0) + 1);
```

```java
if (map.containsKey(x)) {
}
```

```java
int value = map.get(x);
```

```java
map.put(x, i);
```

```java
map.computeIfAbsent(key, k -> new ArrayList<>()).add(value);
```

---

# 36. What I'd learn in order

Since you're preparing for **Java DSA**, I'd do HashMap in this order:

```text
1. HashMap basics
       ↓
2. HashSet
       ↓
3. Frequency counting
       ↓
4. Two Sum
       ↓
5. Anagrams
       ↓
6. HashMap + Sliding Window
       ↓
7. HashMap + Prefix Sum
       ↓
8. Grouping / computeIfAbsent
       ↓
9. Custom objects + equals/hashCode
       ↓
10. LRU Cache / LFU Cache
```

And don't move to LRU/LFU until the first **7–8 patterns feel automatic**.

### Your core template to remember

```java
HashMap<K, V> map = new HashMap<>();

for (...) {

    // What information do I need?
    // K = thing I'm looking up
    // V = information I need to remember

    if (map.containsKey(key)) {
        // use previous information
    }

    map.put(key, value);
}
```

If you master that mental model, **HashMap stops being a Java API topic and becomes a DSA pattern/tool**.