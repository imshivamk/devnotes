### 1. Java Basic Input/Output

#### Introduction

In Java, all execution happens inside classes. The entry point of any Java program is the `main` method. Input and output are handled using standard classes and streams from the `java.lang` and `java.util` packages.

  

#### Basic Program Structure & Packages

Packages provide pre-built classes and tools.

  

- `import java.util.Scanner;` → used for input/output.
    
      
    
- `java.lang.Math` → automatically available for mathematical functions.
    
      
    

Java

```java
public class Main {
    public static void main(String[] args) {
        // Your code here
    }
}
```

#### Output with `System.out.print`

To print output without adding a new line, use `System.out.print()`.

  

Java

```java
public class Main {
    public static void main(String[] args) {
        System.out.print("Hey, Striver!");
    }
}
```

**Output:**

  

```
Hey, Striver!
```

#### Printing on Multiple Lines

Calling `System.out.print()` repeatedly prints on the same line:

  

Java

```java
public class Main {
    public static void main(String[] args) {
        System.out.print("Hey, Striver!");
        System.out.print("Hey, Striver!");
    }
}
```

**Output:**

  

```
Hey, Striver!Hey, Striver!
```

To move to a new line, either append `\n` or use `System.out.println()`:

  

Java

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hey, Striver!");
        System.out.print("Hey, Striver!");
    }
}
```

**Output:**

  

```
Hey, Striver!
Hey, Striver!
```

- `\n` → newline escape character inside a string.
    
      
    
- `System.out.println()` → prints the line and automatically adds a platform-specific newline.
    
      
    

#### Taking User Input with `Scanner`

To read user input, create a `Scanner` instance linked to standard input (`System.in`).

  

Java

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int x = sc.nextInt();
        System.out.println("Value of x: " + x);
    }
}
```

**Input:** `10`

  

**Output:** `Value of x: 10`

  

#### Multiple Inputs

To read multiple values, chain `sc.nextInt()` (or other data-type readers like `nextDouble()`, `next()`).

  

Java

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int x = sc.nextInt();
        int y = sc.nextInt();
        System.out.println("Value of x: " + x + " and y: " + y);
    }
}
```

**Input:** `10 20`

  

**Output:** `Value of x: 10 and y: 20`

  

#### Wildcard Import Shortcut

Instead of importing individual classes, you can import entire packages using `*`:

  

Java

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int x = sc.nextInt();
        int y = sc.nextInt();
        System.out.println("Value of x: " + x + " and y: " + y);
    }
}
```

- `import java.util.*;` imports all classes within the `java.util` package at once.
    
      
    
- Common in competitive programming; explicit imports are preferred in production to prevent naming collisions.


### 2. Java Arrays and Strings

#### What is an Array?

An array is a linear data structure that stores a collection of homogeneous (identical data type) elements in contiguous, adjacent memory locations.

  

- **Random Access:** Any element can be accessed directly in constant time $O(1)$ using its index.
    
      
    
- **Contiguous Memory:** Because elements are stored in back-to-back memory blocks, the computer computes the exact memory address instantly:
    
      
    
    $$\text{Address} = \text{Base Address} + (\text{Index} \times \text{Size of Data Type})$$
    
- **Homogeneous:** An array can store primitives (`int`, `double`, `boolean`, `char`) or object references (`String`, custom classes), but all elements in a single array must share the declared type. Heterogeneous (mixed) types cannot be stored in a standard array.
    
      
    

#### Why are Arrays 0-Indexed?

Starting at index `0` simplifies memory address computation. The index represents the **offset** (distance) from the array's base address:

  

- Index `0` $\rightarrow$ Offset of $0$ elements from the base memory address.
    
      
    
- Index `i` $\rightarrow$ Offset of $i$ elements from the base memory address.
    
      
    

For an array of length $n$, valid indices range from $0$ to $n - 1$.

  

#### Creating and Defining an Array

In Java, arrays are dynamically allocated objects on the heap using the `new` keyword. Once initialized, the size is fixed and cannot be changed.

  

**Syntax:**

  

Java

```java
dataType[] arrayName = new dataType[arraySize];
```

Java

```java
public class Main {
    public static void main(String[] args) {
        // Declares and allocates space for 8 integers (default initialized to 0)
        int[] myArray = new int[8];

        // Assigning values via 0-based index
        myArray[0] = 10;
        myArray[1] = 20;

        // Direct initialization syntax
        int[] numbers = {10, 20, 30, 40, 50};

        System.out.println("First element: " + numbers[0]);
        System.out.println("Array length: " + numbers.length); // .length is a property, not a method
    }
}
```

#### Finding an Element in an Array

- **Direct Access:** If the index is known, access is instant: $O(1)$.
    
      
    
- **Search Algorithms:** If the index is unknown, use Linear Search ($O(n)$) on unsorted arrays or Binary Search ($O(\log n)$) on sorted arrays (`Arrays.binarySearch()`).
    
      
    
- **Hash-Based Lookup:** For repeated, fast lookups ($O(1)$ average), store elements in a `HashSet` or `HashMap`.
    
      
    

#### Key Characteristics & Operations Summary

- **Memory Allocation:** Memory is allocated when the array is instantiated with `new`. Primitives receive default values (`0` for numeric types, `false` for `boolean`, `null` for objects).
    
      
    
- **Contiguous Storage:** Enables direct $O(1)$ indexing.
    
      
    
- **Insertion & Deletion:**
    
      
    - In fixed-size arrays, "inserting" or "deleting" requires shifting subsequent elements left or right ($O(n)$ time for arbitrary positions).
        
          
        
    - Dynamic collections like `ArrayList` handle dynamic resizing automatically.
        
          
        

#### What are Strings?

A `String` in Java is an object from the `java.lang` package representing an ordered sequence of characters. It is 0-indexed, where the first character is at index `0` and the last character is at index `length() - 1`.

  

#### 1. Finding the Length of a String

To get the number of characters in a string, call the `.length()` method. _(Note: Arrays use `.length` as a property, whereas Strings use `.length()` as a method)._

  

Java

```java
public class Solution {
    public int findLength(String s) {
        return s.length();
    }

    public static void main(String[] args) {
        Solution obj = new Solution();
        String s = "Hello World";
        System.out.println(obj.findLength(s));
    }
}
```

**Output:**

  

```
11
```

#### 2. Accessing Individual Characters

Java strings do not support array subscript syntax (e.g., `s[i]`). Instead, use the `.charAt(int index)` method.

  

Java

```java
public class Solution {
    public void accessCharacters(String s) {
        for (int i = 0; i < s.length(); i++) {
            System.out.println(s.charAt(i));
        }
    }

    public static void main(String[] args) {
        Solution obj = new Solution();
        String s = "Hello";
        obj.accessCharacters(s);
    }
}
```

**Output:**

  

```
H
e
l
l
o
```

#### 3. String Immutability and Modification

In Java, `String` objects are **immutable**—their contents cannot be modified in place once created in memory.

  

- Assigning or passing a string passes a copy of the reference, not a deep copy of the underlying characters.
    
      
    
- Any modification creates a completely new `String` object.
    
      
    
- To perform efficient mutations or concatenations without creating redundant intermediate objects, use `StringBuilder` or convert the string to a `char[]`.
    
      
    

Java

```java
public class Solution {
    public String modifyString(String str) {
        // Convert the immutable String to a mutable char array
        char[] charArray = str.toCharArray();

        // Modify the first character
        charArray[0] = 'H';

        // Return a newly constructed String
        return new String(charArray);
    }

    public static void main(String[] args) {
        String original = "hello";

        Solution sol = new Solution();
        String modified = sol.modifyString(original);

        System.out.println("Original String: " + original);
        System.out.println("Modified String: " + modified);
    }
}
```

**Output:**

  

```
Original String: hello
Modified String: Hello
```

#### 4. String Comparison

In Java, comparing strings requires specific methods depending on what is being compared:

  

- **`==` Operator:** Checks **reference equality** (whether both references point to the exact same memory address).
    
      
    
- **`.equals()` Method:** Checks **content / value equality** (whether both strings have identical character sequences).
    
      
    
- **`!str1.equals(str2)`:** Checks for inequality in string content.
    

Java

```java
import java.util.Scanner;

public class Solution {
    public boolean compareStrings(String str1, String str2) {
        // Checks whether string contents are identical
        return str1.equals(str2);
    }

    public static void main(String[] args) {
        Solution obj = new Solution();
        Scanner sc = new Scanner(System.in);

        String str1 = sc.next();
        String str2 = sc.next();

        if (obj.compareStrings(str1, str2)) {
            System.out.println("Strings are equal");
        } else {
            System.out.println("Strings are not equal");
        }
    }
}
```



### 3. Java If-Else Statements

#### Introduction to Conditional Statements

Conditional statements allow decision-making based on dynamic runtime conditions. They execute different code blocks depending on whether a boolean expression evaluates to `true` or `false`.

  

#### The `if-else` Statement

- **`if` statement:** Executes its enclosed block of code only when the specified condition evaluates to `true`.
    
      
    
- **`else` statement:** An optional companion block that executes when the condition evaluates to `false`.
    
      
    

**Flow of Control:**

  

- If the boolean condition inside `if(...)` evaluates to `true`, the `if` block executes.
    
      
    
- If the boolean condition evaluates to `false`, control passes to the `else` block (if provided).
    
      
    

Java

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // Ask the user to enter their age
        System.out.print("Enter your age: ");
        int age = sc.nextInt();

        // Check if the user is 18 or older
        if (age >= 18) {
            // If true, print that the user is an adult
            System.out.println("You are an adult.");
        } else {
            // Otherwise, print that the user is not an adult
            System.out.println("You are not an adult.");
        }
    }
}
```

#### Simplifying Multi-Condition Logic with `else if`

When checking multiple ranges or conditions, an `else if` ladder structures sequential decision paths instead of requiring deeply nested `if` blocks.

  

**Grading Example with Explicit Bounds:**

  

Java

```java
public class Main {
    public static void main(String[] args) {
        int marks = 54;

        // Check grade conditions using if-else ladder
        if (marks < 25) {
            System.out.println("Grade: F");
        } else if (marks >= 25 && marks <= 44) {
            System.out.println("Grade: E");
        } else if (marks >= 45 && marks <= 49) {
            System.out.println("Grade: D");
        } else if (marks >= 50 && marks <= 59) {
            System.out.println("Grade: C");
        } else if (marks >= 60 && marks <= 69) {
            System.out.println("Grade: B");
        } else if (marks >= 70 && marks <= 100) {
            System.out.println("Grade: A");
        } else {
            System.out.println("Invalid marks entered.");
        }
    }
}
```

#### Optimized `if-else` Ladder (Removing Redundant Comparisons)

Because conditions in an `if-else if` ladder are evaluated sequentially from top to bottom, reaching an `else if` block guarantees that all preceding conditions evaluated to `false`. Lower-bound checks are redundant and can be omitted.

Java

```java
public class Main {
    public static void main(String[] args) {
        int marks = 54;

        // Optimized sequential ladder
        if (marks < 0 || marks > 100) {
            System.out.println("Invalid marks entered.");
        } else if (marks < 25) {
            System.out.println("Grade: F");
        } else if (marks <= 44) {
            System.out.println("Grade: E");
        } else if (marks <= 49) {
            System.out.println("Grade: D");
        } else if (marks <= 59) {
            System.out.println("Grade: C");
        } else if (marks <= 69) {
            System.out.println("Grade: B");
        } else {
            System.out.println("Grade: A");
        }
    }
}
```


### 4. Java Switch-Case Statements

#### Introduction to Switch Statements

While `if-else` ladders are suited for handling complex boolean conditions and range evaluations, `switch` statements provide a cleaner, more structured alternative when comparing a single variable against multiple discrete, constant values.

  

#### The Basic `switch` Statement

A `switch` evaluates an expression and jumps directly to the corresponding `case` label.

  

- **`break` statement:** Terminates the `switch` block. Without a `break`, execution continues down into subsequent cases regardless of their condition (**fall-through behavior**).
    
      
    
- **`default` label:** An optional safety net that executes if none of the explicit `case` values match the expression.
    
      
    

Java

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // Prompt user to enter a number between 1 and 7
        System.out.print("Enter a number (1-7): ");
        int day = sc.nextInt();

        // Switch statement to determine the day
        switch (day) {
            case 1:
                System.out.println("Monday");
                break; // Exit switch after matching case
            case 2:
                System.out.println("Tuesday");
                break;
            case 3:
                System.out.println("Wednesday");
                break;
            case 4:
                System.out.println("Thursday");
                break;
            case 5:
                System.out.println("Friday");
                break;
            case 6:
                System.out.println("Saturday");
                break;
            case 7:
                System.out.println("Sunday");
                break;
            default:
                // Executed if input is not between 1 and 7
                System.out.println("Invalid");
                break;
        }
    }
}
```

#### Key Considerations for Switch Statements

##### 1. Requirement for Constant Expressions

Each `case` label must be a **compile-time constant** expression or literal. Variables used inside `case` labels must be marked `final`.

  

Java

```java
public class Main {
    public static void main(String[] args) {
        // Define constant integers using final
        final int x = 10;
        final int y = 5;

        // Evaluate expression
        switch (x + y) {
            case 15: // Constant expression match
                System.out.println("Result is 15.");
                break;
            case 20:
                System.out.println("Result is 20.");
                break;
            default:
                System.out.println("No match found.");
                break;
        }
    }
}
```

##### 2. Supported Data Types

In Java, `switch` supports the following types:

  

- Primitive integral types: `byte`, `short`, `char`, `int` (and their corresponding Wrapper classes: `Byte`, `Short`, `Character`, `Integer`).
    
      
    
- `String` (supported since Java 7).
    
      
    
- `enum` types.
    
      
    
- _(Note: Floating-point types like `float`, `double`, and `boolean` are not supported)._
    
      
    

Java

```java
public class Main {
    public static void main(String[] args) {
        // Character variable representing a grade
        char grade = 'B';

        // Evaluate grade using switch
        switch (grade) {
            case 'A':
                System.out.println("Excellent!");
                break;
            case 'B':
                System.out.println("Good!");
                break;
            default:
                System.out.println("Not specified.");
                break;
        }
    }
}
```

#### Modern Java Enhanced Switch (Arrow Syntax)

In modern Java (Java 14+), the enhanced `switch` eliminates the need for manual `break` statements and prevents accidental fall-through using arrow (`->`) syntax:

Java

```java
public class Main {
    public static void main(String[] args) {
        int day = 4;

        // Modern enhanced switch expression/statement
        switch (day) {
            case 1 -> System.out.println("Monday");
            case 2 -> System.out.println("Tuesday");
            case 3 -> System.out.println("Wednesday");
            case 4 -> System.out.println("Thursday");
            case 5 -> System.out.println("Friday");
            case 6 -> System.out.println("Saturday");
            case 7 -> System.out.println("Sunday");
            default -> System.out.println("Invalid");
        }
    }
}
```


### 5. Java Loops

#### Introduction to While Loops

A `while` loop is an entry-controlled control structure that repeatedly executes a block of code as long as a specified boolean condition evaluates to `true`.

  

If the condition is `false` initially, the loop body will not execute even once.

  

#### Step-by-Step Flow of Execution

1. **Evaluation of Test Expression:** The condition inside `while(...)` is evaluated before entering the loop.
    
      
    
2. **Condition Check:**
    
      
    - If `true`, the statements within the loop body execute.
        
          
        
    - If `false`, the loop terminates immediately, and execution jumps to the code following the loop.
        
          
        
3. **Re-evaluation:** After executing the body statements, the condition is evaluated again.
    
      
    
4. **Continuation or Termination:** This cycle repeats until the condition evaluates to `false`.
    
      
    

#### Practical Example: Finding Factorial

The factorial of a number $n$ ($n!$) is the product of all positive integers from $1$ to $n$. Using a `while` loop, we initialize `factorial` to `1` and multiply it by `n` while decrementing `n` on each iteration until `n` reaches `0`.

  

Java

```java
public class Main {
    public static void main(String[] args) {
        int n = 5;
        int factorial = 1;

        while (n > 0) {
            factorial *= n;  // Multiply factorial by n
            n--;             // Decrement n
        }

        System.out.println("Factorial of 5 is: " + factorial);  // Print the result
    }
}
```

**Output:**

  

```
Factorial of 5 is: 120
```

#### When to Use a While Loop

- **Unknown Iteration Count:** When the exact number of iterations is not known in advance and depends on dynamic runtime conditions (e.g., processing stream data, reading until EOF, or waiting for specific user input).
    
      
    
- **Pre-condition Validation:** When you must verify that prerequisites are met before running the block even once.

#### What is a For Loop and Why is it Used?

A `for` loop is a control flow structure that repeatedly executes a block of code based on a specified condition. It is ideal when the number of iterations is known beforehand.

  

A standard `for` loop consists of three main components:

  

1. **Initialization:** Declares and initializes the loop control counter variable (runs once at the beginning).
    
      
    
2. **Condition:** Evaluated before every iteration. If `true`, the loop body executes; if `false`, the loop terminates.
    
      
    
3. **Increment/Decrement (Update):** Modifies the counter variable after each iteration before the next condition check.
    
      
    

Java

```java
public class Main {
    public static void main(String[] args) {
        for (int i = 1; i <= 10; i++) {
            System.out.println("Hey, Striver, this is the " + i + "'th iteration");
        }
    }
}
```

#### Flow of Execution

- **Start:** The loop begins execution.
    
      
    
- **Initialization:** The counter variable is initialized (e.g., `int i = 1`).
    
      
    
- **Condition Check:** The condition (`i <= 10`) is evaluated. If `true`, execution moves to the body; if `false`, the loop exits.
    
      
    
- **Body Execution:** The code block within `{ ... }` executes.
    
      
    
- **Counter Update:** The increment/decrement step runs (e.g., `i++`).
    
      
    
- **Next Iteration:** Control loops back to the **Condition Check**.
    
      
    
- **Exit:** Once the condition evaluates to `false`, control continues to the code immediately following the loop.
    
      
    

#### Nested For Loops

A `for` loop can be placed inside another `for` loop. The inner loop completes all its iterations for every single cycle of the outer loop. This is essential for traversing 2D arrays, matrices, and generating nested patterns.

  

Java

```java
public class Main {
    public static void main(String[] args) {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                System.out.println("i = " + i + ", j = " + j);
            }
        }
    }
}
```

#### Conditionals Inside For Loops

Branching logic like `if-else` can be embedded inside a `for` loop to filter or handle specific iterations differently.

  

Java

```java
public class Main {
    public static void main(String[] args) {
        for (int i = 1; i <= 10; i++) {
            if (i % 2 == 0) {
                System.out.println(i + " is Even");
            } else {
                System.out.println(i + " is Odd");
            }
        }
    }
}
```

#### Customizing For Loops

The update step is not limited to increments of 1 (`i++`). You can use any valid arithmetic assignment, such as `i += 5`, `i *= 2`, or decrements like `i--`.

Java

```java
public class Main {
    public static void main(String[] args) {
        for (int i = 1; i <= 25; i += 5) {
            System.out.println("i = " + i);
        }
    }
}
```

### 6. Java Functions (Pass by Value and Reference Simulation)

#### Analogy: Pass by Value vs. Pass by Reference

- **Pass by Value:** You make a photocopy of a document and make edits on the photocopy. The original document remains untouched.
    
      
    
- **Pass by Reference:** You hand over the original document and make edits directly on it. The original document is modified.
    
      
    

#### What is Pass by Value?

When a variable is passed by value, a copy of the actual data is passed to the method's local parameter.

  

- The method operates strictly on the copied value.
    
      
    
- Modifications made inside the method do not affect the original caller's variable.
    
      
    
- Ensures caller data safety.
    
      
    

#### What is Pass by Reference?

When a variable is passed by reference, the method receives the direct memory reference (address) of the variable.

  

- Any operation or modification performed inside the method directly updates the original variable.
    
      
    
- Multiple variables or state changes can be reflected back to the caller without explicit `return` statements.
    
      
    

#### Pass by Value vs. Pass by Reference

|**Feature**|**Pass by Value**|**Pass by Reference**|
|---|---|---|
|**What is passed?**|A copy of the value/bits|The direct memory address|
|**Original data modified?**|No|Yes|
|**Primary Use Case**|Data safety and isolation|In-place modification and memory efficiency|
|**Memory Impact**|Allocates extra space for copies|Reuses the existing memory address|

#### How Java Handles Parameter Passing

**Java is strictly pass-by-value at all times.** There is no true pass-by-reference in Java.

  

- **Primitives (`int`, `double`, `char`, `boolean`):** The exact binary value is copied. The original variable is completely isolated from changes.
    
      
    
- **Objects & Arrays:** The value of the **reference pointer** (the memory address of the object on the heap) is copied. Because both the caller's variable and the method parameter point to the same underlying heap object, modifying the object's internal fields or array elements affects the original object. However, reassigning the reference variable itself to a `new` object does not affect the caller's reference.
    
      
    

#### 1. Primitives (Pass by Value)

Passing primitive types creates an independent copy on the call stack.

  

Java

```java
public class Main {
    static void modify(int a) {
        a = a + 10; // Changes local copy only
    }

    public static void main(String[] args) {
        int x = 5;
        modify(x);
        System.out.println("Value of x: " + x); // Output: 5
    }
}
```

**Output:**

  

```
Value of x: 5
```

#### 2. Objects (Passing Reference by Value)

Passing an object copies its reference. Modifying internal state alters the shared object on the heap.

  

Java

```java
class Wrapper {
    int value = 5;
}

public class Main {
    static void modify(Wrapper obj) {
        obj.value += 10; // Modifies the heap object directly
    }

    public static void main(String[] args) {
        Wrapper w = new Wrapper();
        modify(w);
        System.out.println("Value of w.value: " + w.value); // Output: 15
    }
}
```

**Output:**

  

```
Value of w.value: 15
```

#### 3. Proving Java is Not Pass by Reference (Reference Reassignment)

If Java supported true pass-by-reference, reassigning `obj = new Wrapper(...)` inside a method would overwrite the caller's original object pointer. In Java, it only rebinds the local copy of the reference.

  

Java

```java
class Wrapper {
    int value = 5;
}

public class Main {
    static void reassign(Wrapper obj) {
        obj = new Wrapper(); // Points local variable to a new object
        obj.value = 50;
    }

    public static void main(String[] args) {
        Wrapper w = new Wrapper();
        reassign(w);
        System.out.println("Value of w.value after reassign: " + w.value); // Still 5
    }
}
```

**Output:**

```
Value of w.value after reassign: 5
```

### 7. Time and Space Complexity in Java

#### What is Time Complexity?

Time complexity measures how the execution time of an algorithm grows as the input size ($N$) increases. It does not measure physical clock time (in seconds or milliseconds), because real-world wall-clock time varies significantly depending on hardware specifications, processor load, operating system, and JVM runtime optimizations.

  

**Definition:**

Time complexity is the mathematical rate at which the computational operations required by a program scale relative to the input size ($N$).

  

#### Representing Time Complexity: Asymptotic Notations

- **Big $O$ Notation ($O$):** Represents the **Upper Bound** (worst-case scenario). This guarantees that the algorithm will perform no worse than this rate.
    
      
    
- **Theta Notation ($\Theta$):** Represents the **Tight Bound** (average case). The algorithm runs within this bound from both above and below.
    
      
    
- **Omega Notation ($\Omega$):** Represents the **Lower Bound** (best-case scenario). The algorithm will require at least this many operations.
    
      
    

|**Notation**|**Bound Represented**|**Case Analyzed**|
|---|---|---|
|**Big $O$ ($O$)**|Upper Bound|Worst Case|
|**Theta ($\Theta$)**|Tight Bound|Average Case|
|**Omega ($\Omega$)**|Lower Bound|Best Case|

#### 3 Core Rules for Calculating Big $O$

1. **Focus on the Worst-Case Scenario:** Always evaluate algorithm performance under the least favorable inputs to ensure system robustness and reliability.
    
      
    
2. **Drop Constant Terms:** Fixed steps that execute in constant time $O(1)$ do not scale with $N$ and are omitted. For example, $O(3N + 1) \rightarrow O(3N)$.
    
      
    
3. **Drop Non-Dominant / Lower-Order Terms & Coefficients:** As $N$ approaches large values ($N \rightarrow \infty$), lower-degree terms and constant multipliers become negligible:
    
      
    
    $$O(4N^3 + 3N^2 + 8) \rightarrow O(4N^3) \rightarrow O(N^3)$$
    

#### Analyzing Java Code Examples

##### Example 1: Single Loop Operations

Java

```
public class Main {
    public static void main(String[] args) {
        int n = 5;
        for (int i = 1; i <= n; i++) {
            System.out.println("Raj");
        }
    }
}
```

- **Analysis:** The loop executes $N$ times. In each iteration, fixed operations occur (comparison, printing, increment).
    
      
    
- **Total Operations:** $\approx 3 \times N$ steps.
    
      
    
- **Time Complexity:** $O(N)$ (linear time).
    
      
    

##### Example 2: Nested Loops ($N \times N$)

Java

```
public class Main {
    public static void main(String[] args) {
        int n = 5;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                // Constant time operation
                System.out.println("i = " + i + ", j = " + j);
            }
        }
    }
}
```

- **Analysis:** The outer loop executes $N$ times. For every iteration of the outer loop, the inner loop runs $N$ times.
    
      
    
- **Total Operations:** $N + N + N + \dots + N = N \times N = N^2$.
    
      
    
- **Time Complexity:** $O(N^2)$ (quadratic time).
    
      
    

##### Example 3: Dependent Nested Loops ($1 + 2 + \dots + N$)

Java

```
public class Main {
    public static void main(String[] args) {
        int n = 5;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j <= i; j++) {
                // Constant time operation
                System.out.println("Processing: " + i + ", " + j);
            }
        }
    }
}
```

- **Analysis:**
    
      
    - When $i = 0$, inner loop runs $1$ time.
        
          
        
    - When $i = 1$, inner loop runs $2$ times.
        
          
        
    - When $i = N - 1$, inner loop runs $N$ times.
        
          
        
- **Total Operations:**
    
      
    
    $$1 + 2 + 3 + \dots + N = \frac{N(N + 1)}{2} = \frac{N^2}{2} + \frac{N}{2}$$
    
- **Dropping lower-order terms and coefficients:**
    
      
    
    $$O\left(\frac{N^2}{2} + \frac{N}{2}\right) \rightarrow O(N^2)$$
    
- **Time Complexity:** $O(N^2)$.
    
      
    

#### What is Space Complexity?

Space complexity measures the total memory required by an algorithm to execute to completion relative to the input size $N$.

  

$$\text{Space Complexity} = \text{Input Space} + \text{Auxiliary Space}$$

- **Input Space:** Memory used to store the input data given to the algorithm.
    
      
    
- **Auxiliary Space:** Additional or temporary memory allocated specifically to solve the problem (e.g., extra variables, temporary arrays, recursion stack).
    
      
    

Java

```
public class Main {
    public static void main(String[] args) {
        int a = 10; // Input Space
        int b = 20; // Input Space
        int c = a + b; // Auxiliary Space (temporary variable)

        System.out.println("Sum: " + c);
    }
}
```

- **Space Complexity:** $O(1)$ auxiliary space (constant space).
    
      
    

#### Good Coding Practice: Data Immutability

Avoid mutating or overwriting the original input variables (e.g., `b = a + b`) merely to save auxiliary variables unless explicitly permitted by the problem statement or interviewer. Preserving the integrity of original data prevents side effects in larger systems.

  

#### Online Judges & The $10^8$ Operations Rule

Online competitive programming judges (e.g., LeetCode, Codeforces) allocate roughly **$1$ second per test suite execution**, which corresponds to approximately **$10^8$ basic machine operations**.

|**Input Size (N)**|**Required Worst-Case Time Complexity**|**Feasible Operations (1 sec≈108)**|
|---|---|---|
|$N \le 10 \text{ to } 12$|$O(N!)$ or $O(2^N \cdot N)$|Permutations, Backtracking|
|$N \le 20 \text{ to } 25$|$O(2^N)$|Bitmask DP, Subsets|
|$N \le 500$|$O(N^3)$|Floyd-Warshall, Matrix Operations|
|$N \le 5000$|$O(N^2)$|Nested loops, Dynamic Programming|
|$N \le 10^5 \text{ to } 10^6$|$O(N \log N)$ or $O(N)$|Sorting, Segment Trees, Two Pointers|
|$N \le 10^9 \text{ to } 10^{18}$|$O(\log N)$ or $O(1)$|Binary Search, Math, Modular Exponentiation|