
## 1. What is a HashMap?

A `HashMap` is a data structure that stores data as **key-value pairs**.

Example:

```java
HashMap<Integer, String> students = new HashMap<>();

students.put(101, "Shivam");
students.put(102, "Rahul");
students.put(103, "Aman");
```

Internally:

```text
101 -> Shivam
102 -> Rahul
103 -> Aman
```

Think of it like a dictionary:

```text
Word      Meaning
Dog   ->  Animal
Cat   ->  Animal
Java  ->  Programming Language
```

The word is the **key** and the meaning is the **value**.

---

## 2. Why Use HashMap?

Suppose you have:

```java
Student ID -> Student Name
```

Without HashMap:

```java
int[] ids = {101,102,103};
String[] names = {"Shivam","Rahul","Aman"};
```

To find:

```java
ID = 103
```

You must search through the array.

Time Complexity:

```text
O(n)
```

With HashMap:

```java
map.get(103);
```

Time Complexity:

```text
O(1) average
```

Very fast lookup.

---

## 3. Importing HashMap

```java
import java.util.HashMap;
```

---

## 4. Creating a HashMap

### Syntax

```java
HashMap<KeyType, ValueType> map = new HashMap<>();
```

Example:

```java
HashMap<Integer, String> map = new HashMap<>();
```

Key:

```java
Integer
```

Value:

```java
String
```

---

## 5. Important Properties

### Keys Must Be Unique

```java
map.put(1, "Shivam");
map.put(1, "Rahul");
```

Result:

```text
1 -> Rahul
```

Old value replaced.

---

### Values Can Be Duplicate

```java
map.put(1, "Java");
map.put(2, "Java");
```

Valid.

---

### Only One Null Key

```java
map.put(null, "ABC");
```

Valid.

```java
map.put(null, "XYZ");
```

Replaces previous value.

---

### Multiple Null Values Allowed

```java
map.put(1, null);
map.put(2, null);
```

Valid.

---

## 6. Basic Operations

### put()

Insert data.

```java
map.put(101, "Shivam");
map.put(102, "Rahul");
```

---

### get()

Retrieve value using key.

```java
String name = map.get(101);
System.out.println(name);
```

Output:

```text
Shivam
```

---

### remove()

Delete entry.

```java
map.remove(101);
```

---

### containsKey()

Check key exists.

```java
map.containsKey(101);
```

Returns:

```java
true
```

or

```java
false
```

---

### containsValue()

```java
map.containsValue("Shivam");
```

---

### size()

```java
System.out.println(map.size());
```

---

### isEmpty()

```java
map.isEmpty();
```

---

### clear()

Removes everything.

```java
map.clear();
```

---

## 7. Traversing a HashMap

### Method 1: keySet()

```java
for(Integer key : map.keySet()){
    System.out.println(key);
}
```

---

### Method 2: Values

```java
for(String value : map.values()){
    System.out.println(value);
}
```

---

### Method 3: Key + Value

```java
for(Integer key : map.keySet()){
    System.out.println(key + " " + map.get(key));
}
```

---

### Method 4: entrySet() (Most Efficient)

```java
for(Map.Entry<Integer,String> entry : map.entrySet()){

    System.out.println(
        entry.getKey() +
        " " +
        entry.getValue()
    );
}
```

Output:

```text
101 Shivam
102 Rahul
103 Aman
```

---

## 8. Internal Working of HashMap

This is the most important interview topic.

### HashMap Internally Uses

```text
Array + Linked List + Red Black Tree
```

(Java 8+)

---

### Example

```java
map.put(101, "Shivam");
```

#### Step 1

HashMap calculates:

```java
hashCode()
```

of key.

```text
101 -> hash value
```

---

#### Step 2

Hash value determines bucket index.

```text
bucket = hash % array_size
```

Example:

```text
101 -> Bucket 5
```

---

#### Step 3

Store key-value pair in that bucket.

```text
Bucket 5
---------
101 -> Shivam
```

---

## 9. Collision

### What is Collision?

Two keys generate same bucket.

Example:

```text
101 -> Bucket 5
201 -> Bucket 5
```

Now:

```text
Bucket 5

101 -> Shivam
201 -> Rahul
```

Both stored in same bucket.

This is called:

```text
Hash Collision
```

---

## 10. Collision Handling

### Before Java 8

Used Linked List.

```text
Bucket 5

101 -> Shivam
   |
201 -> Rahul
   |
301 -> Aman
```

Search:

```text
O(n)
```

Worst case.

---

### Java 8+

When entries in bucket exceed threshold:

```text
8
```

Linked List becomes:

```text
Red Black Tree
```

Search becomes:

```text
O(log n)
```

---

## 11. Load Factor

Load factor controls resizing.

Default:

```java
0.75
```

Meaning:

```text
75% full
```

then resize.

---

### Example

Initial capacity:

```text
16
```

Load Factor:

```text
0.75
```

Threshold:

```text
16 × 0.75 = 12
```

After 12 elements:

```text
Resize
```

New capacity:

```text
32
```

---

## 12. Rehashing

When capacity increases:

```text
16 -> 32
```

all elements must be placed again.

Called:

```text
Rehashing
```

Expensive operation.

---

## 13. Time Complexity

|Operation|Average|Worst|
|---|---|---|
|put()|O(1)|O(n)|
|get()|O(1)|O(n)|
|remove()|O(1)|O(n)|
|containsKey()|O(1)|O(n)|

With treeified buckets:

|Operation|Worst|
|---|---|
|Search|O(log n)|

---

## 14. Initial Capacity

Default:

```java
HashMap<Integer,String> map =
    new HashMap<>();
```

Capacity:

```text
16
```

---

Custom:

```java
HashMap<Integer,String> map =
    new HashMap<>(100);
```

---

Custom Capacity + Load Factor

```java
HashMap<Integer,String> map =
    new HashMap<>(100, 0.75f);
```

---

## 15. HashMap vs Hashtable

|Feature|HashMap|Hashtable|
|---|---|---|
|Thread Safe|No|Yes|
|Faster|Yes|No|
|Null Key|Allowed|Not Allowed|
|Null Value|Allowed|Not Allowed|
|Legacy|No|Yes|

---

## 16. HashMap vs LinkedHashMap

HashMap:

```text
Random Order
```

```java
3 A
1 B
2 C
```

May print in any order.

---

LinkedHashMap:

```text
Insertion Order Preserved
```

```java
1 B
2 C
3 A
```

---

## 17. HashMap vs TreeMap

HashMap:

```text
Unsorted
O(1)
```

TreeMap:

```text
Sorted by Key
O(log n)
```

Example:

```java
TreeMap<Integer,String> map =
    new TreeMap<>();
```

Output:

```text
1
2
3
4
5
```

Automatically sorted.

---

## 18. Common Interview Questions

### Frequency Counter

Count occurrences of elements.

```java
int[] arr = {1,1,2,3,2,1};

HashMap<Integer,Integer> map =
        new HashMap<>();

for(int num : arr){

    map.put(
        num,
        map.getOrDefault(num,0)+1
    );
}

System.out.println(map);
```

Output:

```text
{1=3, 2=2, 3=1}
```

---

### Find First Non-Repeating Character

```java
String str = "aabbcde";

HashMap<Character,Integer> map =
        new HashMap<>();

for(char ch : str.toCharArray()){

    map.put(
        ch,
        map.getOrDefault(ch,0)+1
    );
}

for(char ch : str.toCharArray()){

    if(map.get(ch)==1){
        System.out.println(ch);
        break;
    }
}
```

Output:

```text
c
```

---

## 19. Methods Every Java Developer Should Know

```java
put()
get()
remove()
containsKey()
containsValue()
size()
clear()
isEmpty()
keySet()
values()
entrySet()
getOrDefault()
putIfAbsent()
replace()
computeIfAbsent()
merge()
```

These appear frequently in Spring Boot, backend development, DSA, and interview questions.

---

## Mental Model

Think of a HashMap as:

```text
Array of Buckets
       ↓
Hash Function decides Bucket
       ↓
Store Key-Value Pair
       ↓
Collisions handled using
Linked List / Red Black Tree
```

This combination is why `HashMap` gives near **O(1)** insertion, deletion, and lookup, making it one of the most important data structures for Java interviews, Spring Boot applications, caching, frequency counting, and DSA problems.