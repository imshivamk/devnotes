
## Part 8: Basic Set Theory and Functions

This section covers the collection of objects (sets), how elements within sets associate with one another (relations), and specific rules of association (mappings/functions).

### 1. Set Theory

A set is a well-defined collection of distinct objects.

#### **Core Concepts & Formulas**

- **Representation**:
    
    - **Roaster Form**: Elements are listed individually, e.g., $A = \{1, 2, 3\}$.
        
    - **Set-Builder Form**: Elements are described by a property, e.g., $\{x \mid x \text{ is an even integer}\}$.
        
- **Types of Sets**:
    
    - **Empty Set ($\emptyset$)**: Contains no elements.
        
    - **Subset ($A \subseteq B$)**: Every element of $A$ is also in $B$.
        
    - **Power Set**: The set of all subsets of a set $A$. Total elements $= 2^n$.
        
        +1
        
- **Operations**:
    
    - **Union ($A \cup B$)**: Elements in $A$, or $B$, or both.
        
    - **Intersection ($A \cap B$)**: Elements common to both $A$ and $B$.
        
    - **Complement ($A'$)**: Elements in the Universal set not in $A$.
        
- **De Morgan's Laws**:
    
    - $(A \cup B)' = A' \cap B'$
        
    - $(A \cap B)' = A' \cup B'$
        

> **Solved Example**: If $A = \{1, 2, 3, 4\}$ and $B = \{3, 4, 5, 6\}$, find $A \cup B$ and $A \cap B$.
> 
> **Step 1**: For Union, combine all elements without duplicates.
> 
> **Step 2**: For Intersection, find common elements.
> 
> **Solution**: $A \cup B = \{1, 2, 3, 4, 5, 6\}$; $A \cap B = \{3, 4\}$.

---

### 2. Relations

A relation $R$ from set $A$ to set $B$ is a subset of the Cartesian product $A \times B$.

#### **Core Concepts & Types**

- **Reflexive**: $(a, a) \in R$ for every $a \in A$.
    
- **Symmetric**: If $(a, b) \in R$, then $(b, a) \in R$.
    
- **Transitive**: If $(a, b) \in R$ and $(b, c) \in R$, then $(a, c) \in R$.
    
- **Equivalence Relation**: A relation that is Reflexive, Symmetric, and Transitive.
    

> **Solved Example**: Let $A = \{1, 2, 3\}$. Is $R = \{(1,1), (2,2), (3,3), (1,2), (2,1)\}$ an equivalence relation?
> 
> **Step 1**: Reflexive? Yes, contains (1,1), (2,2), (3,3).
> 
> **Step 2**: Symmetric? Yes, (1,2) is present and so is (2,1).
> 
> **Step 3**: Transitive? Yes, (1,2) and (2,1) exist, and (1,1) also exists.
> 
> **Solution**: Yes, it is an equivalence relation.

---

### 3. Functions (Mappings)

A function is a special relation where every element in the domain has exactly one image in the codomain.

#### **Core Concepts & Formulas**

- **One-to-One (Injective)**: Different elements in the domain have different images.
    
- **Onto (Surjective)**: Every element in the codomain is an image of at least one element in the domain (Range = Codomain).
    
- **Bijective**: A function that is both One-to-One and Onto.
    
- **Inverse Function ($f^{-1}$)**: Exists only if the function is Bijective.
    
- **Composite Function**: $(g \circ f)(x) = g(f(x))$.
    

> **Solved Example**: If $f(x) = 2x + 3$, find the inverse function $f^{-1}(x)$.
> 
> **Step 1**: Set $y = 2x + 3$.
> 
> **Step 2**: Solve for $x$: $y - 3 = 2x \Rightarrow x = (y - 3)/2$.
> 
> **Step 3**: Replace $x$ with $f^{-1}(x)$ and $y$ with $x$.
> 
> **Solution**: $f^{-1}(x) = \frac{x - 3}{2}$.

---

**This concludes the "Mathematics & Statistics" portion of the syllabus.** **When you are ready for the "Logical / Abstract Reasoning" section (Logical situations, Fact-based questions), say "Next".**