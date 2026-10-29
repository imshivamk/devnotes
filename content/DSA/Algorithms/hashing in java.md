# How Hashing Actually Works in Java

Let's start with the mental model:

```text
                    HashMap
┌─────────────────────────────────────────────────────┐
│                                                     │
│ Bucket 0    →  empty                                │
│ Bucket 1    →  [key,value]                          │
│ Bucket 2    →  [key,value] → [key,value]            │
│ Bucket 3    →  empty                                │
│ Bucket 4    →  [key,value]                          │
│ Bucket 5    →  empty                                │
│ ...                                                 │
└─────────────────────────────────────────────────────┘
```

A HashMap is essentially an **array of buckets**.

The hashing process determines **which bucket a key should go into**.

---

# 1. What is a hash?

A hash is a number generated from an object.

In Java, every object ultimately has access to:

```java
hashCode()
```

For example:

```java
String s = "hello";

System.out.println(s.hashCode());
```

You might get:

```text
99162322
```

The important thing is:

```text
"hello"
   ↓
hashCode()
   ↓
99162322
```

That number is the **hash code**.

But here's an important distinction:

> The hash code is NOT the bucket index.

The hash code is used to calculate the bucket.

---

# 2. Why do we need buckets?

Imagine you have:

```text
1,000,000 users
```

and you want to find:

```text
user with ID = 837291
```

Searching one-by-one:

```text
user1
user2
user3
...
user837291
```

would be O(n).

Instead, hashing tries to directly determine where the object belongs.

Conceptually:

```text
837291
   ↓
hash function
   ↓
some number
   ↓
bucket index
   ↓
look there
```

So lookup can be approximately:

```text
O(1)
```

---

# 3. Simple bucket example

Let's pretend our HashMap has only **8 buckets**:

```text
index:

0
1
2
3
4
5
6
7
```

Suppose:

```text
hash = 42
```

We need to convert `42` into one of our 8 bucket indices.

Conceptually:

```text
42 % 8 = 2
```

So:

```text
hash 42
   ↓
bucket 2
```

We put the entry into bucket 2.

Another:

```text
hash = 17

17 % 8 = 1
```

So it goes into bucket 1.

---

# 4. But what if two keys go to the same bucket?

This is a **collision**.

Suppose:

```text
HashMap has 8 buckets
```

And:

```text
key A → hash 42
key B → hash 18
```

Both produce:

```text
42 % 8 = 2
18 % 8 = 2
```

Therefore:

```text
Bucket 2
   ↓
[A]
[B]
```

That's a collision.

And collisions are **completely normal**.

A good hash function minimizes them, but cannot eliminate them completely.

Why?

Because:

```text
Possible keys → enormous
Buckets       → limited
```

Eventually different keys must share a bucket.

This is essentially the **pigeonhole principle**.

---

# 5. How does Java handle collisions?

Historically, Java `HashMap` used a linked list in a bucket:

```text
Bucket 2

    ┌────────────┐
    │ Entry A    │
    └─────┬──────┘
          ↓
    ┌────────────┐
    │ Entry B    │
    └─────┬──────┘
          ↓
    ┌────────────┐
    │ Entry C    │
    └────────────┘
```

Each entry contains roughly:

```text
key
value
hash
next
```

So a bucket can contain multiple entries.

---

# 6. Why store the hash AND the key?

Suppose bucket 2 contains:

```text
apple → 10
banana → 20
orange → 30
```

You ask:

```java
map.get("banana");
```

Java calculates the hash of `"banana"` and determines:

```text
bucket 2
```

Now it looks through bucket 2.

But how does it know which entry is actually `"banana"`?

It compares:

```text
hash
+
equals()
```

Conceptually:

```text
              bucket 2
                 ↓
        ┌─────────────────┐
        │ apple → 10      │
        ├─────────────────┤
        │ banana → 20     │ ← match
        ├─────────────────┤
        │ orange → 30     │
        └─────────────────┘
```

---

# 7. `hashCode()` and `equals()` work together

This is **very important for Java interviews**.

Suppose:

```java
map.put("banana", 20);
```

Java computes:

```text
"banana"
   ↓
hashCode()
   ↓
hash
   ↓
bucket
```

Later:

```java
map.get("banana");
```

Java computes the hash again.

If the bucket matches, it checks equality.

Conceptually:

```java
if (entry.hash == hash &&
    entry.key.equals(key)) {
    
    return entry.value;
}
```

So:

```text
hashCode() → finds the bucket
equals()   → finds the exact key within the bucket
```

That's a fantastic interview answer.

---

# 8. Why can't Java just use `hashCode()`?

Because two different objects can have the same hash.

For example, theoretically:

```text
A → hash 42
B → hash 42
```

Therefore:

```text
hashCode() is NOT guaranteed to be unique
```

So Java needs:

```text
hashCode()
     ↓
find bucket

equals()
     ↓
find exact key
```

---

# 9. The complete `put()` process

Let's walk through:

```java
map.put("Shivam", 100);
```

Conceptually:

### Step 1 — Calculate hash code

```text
"Shivam"
   ↓
hashCode()
   ↓
some integer
```

---

### Step 2 — HashMap processes the hash

Modern Java `HashMap` doesn't simply use the raw `hashCode()` directly. It applies a bit-spreading operation to improve distribution of hash bits.

Conceptually:

```text
key.hashCode()
      ↓
hash spreading
      ↓
internal hash
```

You don't generally need to memorize the exact implementation for DSA.

---

### Step 3 — Determine bucket

The internal hash is mapped to a bucket index.

For a table of length `n`, Java effectively uses a calculation based on:

```text
(n - 1) & hash
```

rather than `% n`.

That's possible because HashMap's table size is maintained as a **power of two**.

For example:

```text
table size = 16

n - 1 = 15
binary  = 1111
```

Then:

```text
hash & 1111
```

gives a number from:

```text
0 → 15
```

So there are 16 possible buckets.

---

# 10. Why power-of-two bucket sizes?

Because bitwise AND is very fast and works neatly with powers of two.

For:

```text
n = 16
```

you have:

```text
n - 1 = 15

15 binary = 1111
```

Therefore:

```text
hash & 15
```

effectively extracts the relevant lower bits.

This is an implementation detail, but it's good interview knowledge.

---

# 11. Step 4 — Is the bucket empty?

Suppose:

```text
bucket 5 = empty
```

Then Java puts the entry there:

```text
Bucket 5

┌──────────────────┐
│ Shivam → 100     │
└──────────────────┘
```

Done.

---

# 12. What if the bucket isn't empty?

Suppose:

```text
Bucket 5

Shivam → 100
```

Now we execute:

```java
map.put("Rahul", 200);
```

and Rahul happens to map to bucket 5.

Java checks the existing entry.

Conceptually:

```text
new key = Rahul

existing key = Shivam

hash equal?
      ↓
   maybe yes

equals?
      ↓
   false
```

Therefore it's a collision.

The new entry gets added to the bucket structure.

Historically:

```text
Bucket 5

Shivam → 100
   ↓
Rahul  → 200
```

---

# 13. What if the key already exists?

Suppose:

```java
map.put("Shivam", 100);
```

and later:

```java
map.put("Shivam", 500);
```

HashMap does **not** create another key.

It finds:

```text
same hash
+
equals() == true
```

and replaces the value:

```text
Before:

Shivam → 100


After:

Shivam → 500
```

That's why:

```java
map.put("A", 10);
map.put("A", 20);
```

produces:

```text
A → 20
```

---

# 14. What happens during `get()`?

Suppose:

```java
map.get("Shivam");
```

The process is approximately:

```text
"Shivam"
    ↓
hashCode()
    ↓
hash spreading
    ↓
bucket index
    ↓
go directly to bucket
    ↓
check entries
    ↓
hash comparison
    ↓
equals()
    ↓
return value
```

So the average case is:

```text
O(1)
```

---

# 15. Why is HashMap lookup NOT always O(1)?

Because of collisions.

Imagine a terrible hash function:

```text
Every key → bucket 0
```

Then:

```text
Bucket 0

A
 ↓
B
 ↓
C
 ↓
D
 ↓
E
 ↓
F
```

Searching for `F` requires traversing entries.

That becomes:

```text
O(n)
```

So the famous statement:

> HashMap lookup is O(1)

really means:

> **Average/expected O(1), assuming reasonably distributed hashes.**

Worst-case behavior is different.

---

# 16. Java 8+ introduced an important optimization: Treeification

This is a very common interview question.

If too many entries collide in the same bucket, Java can transform that bucket from a linked list into a **Red-Black Tree**.

Instead of:

```text
Bucket

A
 ↓
B
 ↓
C
 ↓
D
 ↓
E
 ↓
F
```

it can become conceptually:

```text
             D
           /   \
          B     F
         / \   /
        A   C E
```

Now searching can be approximately:

```text
O(log n)
```

rather than:

```text
O(n)
```

There are thresholds and capacity requirements involved, so don't interpret this as "every collision immediately becomes a tree."

---

# 17. Treeification threshold

A useful Java interview fact:

The default treeification threshold is:

```text
8
```

Meaning when a bucket becomes sufficiently collision-heavy, Java may treeify it.

But there is another important threshold:

```text
MIN_TREEIFY_CAPACITY = 64
```

If the table is too small, HashMap prefers **resizing the table** rather than treeifying the bucket.

So don't say:

> "At 8 collisions HashMap always becomes a tree."

Better:

> "When a bucket becomes sufficiently collision-heavy, Java can treeify it; the default threshold is 8, but the table must also be sufficiently large."

---

# 18. Resizing — another crucial concept

Suppose HashMap has:

```text
16 buckets
```

Initially:

```text
0  1  2  3  4  5  6  7
8  9 10 11 12 13 14 15
```

As you add more elements, buckets become increasingly occupied.

HashMap uses a concept called the **load factor**.

Default:

```text
0.75
```

So for 16 buckets:

```text
16 × 0.75 = 12
```

When the map reaches its resize threshold, it grows.

Typically:

```text
16 → 32 → 64 → 128 → ...
```

---

# 19. Why resize?

Because too many elements relative to buckets means more collisions.

For example:

```text
Before:

16 buckets
12 entries
```

might be reasonably distributed.

But:

```text
16 buckets
100 entries
```

means lots of collisions.

So HashMap increases the number of buckets.

```text
16 buckets
     ↓
resize
     ↓
32 buckets
```

This spreads entries out.

---

# 20. Does resizing change bucket positions?

Yes.

Suppose:

```text
old capacity = 16
```

and an entry previously went to:

```text
bucket 5
```

After:

```text
capacity = 32
```

the bucket calculation can produce a different index.

So entries need to be redistributed/repositioned as part of resizing.

This is why resizing is relatively expensive.

---

# 21. Why is `put()` still O(1) if resizing is expensive?

Because resizing doesn't happen every time.

Most insertions:

```text
O(1)
```

Occasionally:

```text
resize → O(n)
```

But averaged over many operations, insertion remains **amortized O(1)**.

This distinction is useful in interviews:

```text
Average put:        O(1)
Amortized put:      O(1)
Resize operation:   O(n)
```

---

# 22. Visualizing the entire HashMap

Imagine:

```text
HashMap
capacity = 8
```

```text
index
  ↓

0 → null

1 → [apple, 10]

2 → [banana, 20]
          ↓
       [orange, 30]

3 → null

4 → [mango, 40]

5 → null

6 → [grape, 50]

7 → null
```

Notice:

```text
HashMap
   ↓
array of buckets
   ↓
each bucket can contain entries
   ↓
collisions create multiple entries in same bucket
```

Modern Java can represent a heavily-collided bucket as a tree rather than a linked list.

---

# 23. A common misconception

Many beginners think:

```text
HashMap
   ↓
hash function
   ↓
unique bucket
```

That's wrong.

It's:

```text
key
 ↓
hashCode()
 ↓
hash processing
 ↓
bucket index
 ↓
bucket
 ↓
possibly multiple entries
 ↓
hash + equals()
 ↓
exact key
```

There is **no guarantee that one key = one bucket**.

---

# 24. What exactly is an Entry?

Conceptually, a HashMap entry contains information similar to:

```java
class Node<K,V> {
    int hash;
    K key;
    V value;
    Node<K,V> next;
}
```

That's a simplified representation.

So:

```text
Bucket 3
   ↓
┌─────────────────────────┐
│ hash                    │
│ key = "apple"           │
│ value = 100             │
│ next ───────────────────┼──→ next entry
└─────────────────────────┘
```

This is why collisions can be chained.

---

# 25. What happens if you use your own class as a key?

This is where `equals()` and `hashCode()` become extremely important.

Suppose:

```java
class Person {
    String name;

    Person(String name) {
        this.name = name;
    }
}
```

Then:

```java
Person p1 = new Person("Shivam");
Person p2 = new Person("Shivam");

HashMap<Person, Integer> map = new HashMap<>();

map.put(p1, 100);

System.out.println(map.get(p2));
```

You might expect:

```text
100
```

But without overriding `equals()` and `hashCode()`, you shouldn't expect `p1` and `p2` to be considered the same logical key.

Why?

Because they're different objects.

```text
p1 ──→ Person("Shivam")
p2 ──→ Person("Shivam")
```

They have different object identities.

---

# 26. The `equals()` + `hashCode()` contract

The key rule:

If:

```java
a.equals(b) == true
```

then:

```java
a.hashCode() == b.hashCode()
```

**must** be true.

But the reverse isn't required.

You can have:

```text
a.hashCode() == b.hashCode()
```

while:

```text
a.equals(b) == false
```

That's simply a collision.

Think:

```text
same hash
    ↓
maybe same object/key
    ↓
equals() decides
```

---

# 27. The whole thing in one example

Let's pretend:

```java
HashMap<String, Integer> map = new HashMap<>();

map.put("cat", 10);
map.put("dog", 20);
map.put("apple", 30);
```

Internally, conceptually:

```text
"cat"
 ↓
hashCode
 ↓
hash
 ↓
bucket 3
 ↓
[cat → 10]


"dog"
 ↓
hashCode
 ↓
hash
 ↓
bucket 6
 ↓
[dog → 20]


"apple"
 ↓
hashCode
 ↓
hash
 ↓
bucket 3
 ↓
[cat → 10]
      ↓
[apple → 30]
```

`cat` and `apple` collided.

Now:

```java
map.get("apple");
```

does:

```text
"apple"
   ↓
hashCode()
   ↓
bucket 3
   ↓
cat?
   ↓
"cat".equals("apple") → false
   ↓
apple?
   ↓
"apple".equals("apple") → true
   ↓
30
```

That's hashing.

---

# 28. The interview answer you should memorize

If an interviewer asks:

> **"How does HashMap work internally in Java?"**

A strong concise answer is:

> `HashMap` internally uses an array of buckets. When a key is inserted, Java computes the key's `hashCode()`, applies hash processing, and uses it to determine a bucket index. If multiple keys map to the same bucket, a collision occurs. Java handles collisions using a linked structure, and in sufficiently large/heavily-collided buckets it can use a Red-Black Tree. During lookup, HashMap first uses the hash to find the bucket and then uses `equals()` to identify the exact key. The average time complexity for `get` and `put` is O(1), with occasional O(n) resizing operations and tree-based collision handling improving worst-case bucket lookup.`

That's a **very solid Java interview answer**.

---

# 29. The hierarchy you should keep in your head

The easiest way to remember all of this:

```text
                    HashMap
                       │
                       ▼
               Array of buckets
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
       Bucket 0     Bucket 1     Bucket 2
                                     │
                                     ▼
                              collision?
                               /       \
                             no         yes
                             │           │
                             ▼           ▼
                           Entry      multiple
                                      entries
                                         │
                                         ▼
                                  linked structure
                                         │
                              many collisions?
                                         │
                                         ▼
                                  Red-Black Tree
```

And lookup:

```text
key
 │
 ▼
hashCode()
 │
 ▼
hash processing
 │
 ▼
bucket index
 │
 ▼
bucket
 │
 ▼
compare hash
 │
 ▼
equals()
 │
 ▼
value
```

### The four words I'd make automatic:

**Hash → Bucket → Collision → `equals()`**

Once those four are clear, the rest of `HashMap` is mostly implementation detail.