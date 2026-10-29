1. An application offers eight optional tools. In one version, a user selects any three tools for a workspace; in
another, the user places three different tools in three labelled shortcut positions. Count the possibilities in
each version and explain the relationship between the two answers.
2. A laboratory identifier consists of two different uppercase English letters followed by three digits chosen
from 0 to 9. The first digit cannot be zero, but digits may repeat. How many identifiers can be formed?
3. Using the digits 0, 1, 2, 3, 4 and 5 without repetition, how many four-digit even numbers greater than 3000
can be formed?
4. A five-digit access code uses digits from 0 to 9 and may begin with zero. Exactly one digit must occur
twice, and the other three digits must be different from it and from one another. How many valid codes are
possible?
5. Four of eight distinct demonstrations are to be presented in a sequence. A particular accessibility
demonstration must be included, but it cannot be first. How many presentation sequences satisfy this
condition?
6. Seven distinct manuals are arranged on a shelf. A safety handbook must occupy the leftmost position, and
two specified programming manuals must be next to each other. The safety handbook is different from both
programming manuals. Find the number of arrangements.
7. Eight distinct jobs are placed in a processing queue. Job A must occur before Job B, but A must not be
immediately followed by B. How many queues are possible?
8. Find the number of distinct arrangements of all the letters in DATABASE. How many of these
arrangements keep all four vowels together? Copies of the letter A are indistinguishable.
9. A volunteer pool consists of seven coders and five testers, with no person in both categories. A team of five
must contain at least two coders and at least two testers. How many teams can be formed?
10. Four representatives are selected from ten students. Two particular students, Asha and Neel, must either
both be selected or both be left out. How many selections satisfy the condition?
11. A five-member panel is selected from eleven candidates. Candidate E must be included, while candidates C
and D must not both be included. E, C and D are different candidates. How many panels are possible?
12. A question paper contains five questions in Part I and seven in Part II. A student must answer exactly seven
questions, including at least three from each part. In how many ways can the student select the questions?
13. Nine distinct volunteers are divided into three groups of three. Count the divisions when the groups have no
labels. Then count the assignments when the three groups are allocated to registration, technical support and
refreshments, one group to each task.
14. Five distinct software licences are to be installed on five of eight computers, with one licence per selected
computer. Two specified computers must receive licences. How many installations are possible when both
the choice of computers and the allocation of licences matter?
15. A robot moves from (0, 0) to (4, 3), taking only unit steps to the right or upward. How many routes pass
through the checkpoint (2, 1)? Count routes by the positions of the rightward and upward steps.


# solutions

Here are the simplified, intuition-first explanations for each problem, formatted with Obsidian-compatible Markdown math blocks ($$$ \dots $$).

---

### **1. Optional Tools Selection & Ordering**

* **Intuition:**
* In the first version, you just pick a group of tools—the order doesn't matter. This is a **combination**.
* In the second version, you place tools into specific labeled shortcut slots (Slot 1, Slot 2, Slot 3), so order matters. This is a **permutation**.


* **Formulas & Calculations:**
* **Version 1 (Unordered):**

$$\binom{8}{3} = \frac{8!}{3!(8-3)!} = \frac{8 \times 7 \times 6}{3 \times 2 \times 1} = 56$$


* **Version 2 (Ordered):**

$$P(8, 3) = \frac{8!}{(8-3)!} = 8 \times 7 \times 6 = 336$$




* **Relationship:**

$$P(8, 3) = \binom{8}{3} \times 3! \implies 336 = 56 \times 6$$



Once you pick 3 tools, there are $3! = 6$ ways to arrange them into the shortcut slots.

---

### **2. Laboratory Identifier**

* **Intuition:** Break the code into individual slots and multiply the number of choices for each slot using the Multiplication Principle.
* **Approach & Calculation:**
* **2 Letters:** Choose 2 distinct uppercase letters from 26: $26 \times 25 = 650$.
* **First Digit:** Cannot be zero, so digits 1–9 give $9$ choices.
* **Remaining 2 Digits:** Digits 0–9 with repetition allowed give $10 \times 10 = 100$ choices.


$$\text{Total} = (26 \times 25) \times 9 \times 10 \times 10 = 650 \times 900 = 585,000$$



---

### **3. Four-Digit Even Numbers Greater Than 3000**

* **Intuition:** A number is greater than 3000 if its first digit is $3, 4,$ or $5$. It is even if its last digit is $0, 2,$ or $4$. Since digit $4$ affects both ends, we split the work into cases.
* **Approach:**
* **Case 1: First digit is odd ($3$ or $5$)**
* First digit: $2$ choices ($\{3, 5\}$).
* Last digit (even): $3$ choices ($\{0, 2, 4\}$).
* Middle 2 digits: Pick 2 out of the remaining 4 available digits: $P(4, 2) = 12$.
* Ways = $2 \times 3 \times 12 = 72$.


* **Case 2: First digit is even ($4$)**
* First digit: $1$ choice ($\{4\}$).
* Last digit (even): $2$ remaining even choices ($\{0, 2\}$ since $4$ is used).
* Middle 2 digits: $P(4, 2) = 12$.
* Ways = $1 \times 2 \times 12 = 24$.




* **Calculation:**

$$\text{Total} = 72 + 24 = 96$$



---

### **4. Five-Digit Access Code with Repeated Digits**

* **Intuition:** Choose which digit repeats, choose the 3 other distinct digits, and then count how many ways those 5 digits can be arranged.
* **Approach & Calculation:**
1. **Choose repeated digit:** $\binom{10}{1} = 10$ choices.
2. **Choose other 3 distinct digits:** $\binom{9}{3} = 84$ choices.
3. **Arrange the multiset** (where one digit appears twice): $\frac{5!}{2!} = 60$ arrangements.


$$\text{Total} = \binom{10}{1} \times \binom{9}{3} \times \frac{5!}{2!} = 10 \times 84 \times 60 = 50,400$$



---

### **5. Sequence of Four Demonstrations**

* **Intuition:** Place the special accessibility demonstration first (ensuring it's not in slot 1), then fill the remaining slots.
* **Approach & Calculation:**
1. **Accessibility Demo Position:** Can go in slot 2, 3, or 4 ($3$ choices).
2. **Fill remaining 3 slots:** Pick and arrange 3 demos from the remaining 7 distinct demos: $P(7, 3) = 210$.


$$\text{Total} = 3 \times P(7, 3) = 3 \times 210 = 630$$



---

### **6. Arranging Manuals on a Shelf**

* **Intuition:** Lock the safety handbook in position 1, and glue the two programming manuals together so they act as a single block.
* **Approach & Calculation:**
1. **Position 1:** Safety handbook $S$ ($1$ choice).
2. **Remaining Items:** Treat the 2 programming manuals as 1 block. Total items to arrange = $1 \text{ block} + 4 \text{ other manuals} = 5$ items ($5! = 120$ ways).
3. **Internal Order:** The 2 programming manuals can swap places in $2! = 2$ ways.


$$\text{Total} = 1 \times 5! \times 2! = 120 \times 2 = 240$$



---

### **7. Job Queue Order**

* **Intuition:** By symmetry, Job A comes before Job B in exactly half of all total arrangements. From that half, subtract the forbidden cases where Job A is *immediately* followed by Job B.
* **Approach & Calculation:**
1. **A before B (anywhere):** Half of all arrangements of 8 jobs:

$$\frac{8!}{2} = \frac{40,320}{2} = 20,160$$


2. **A immediately before B:** Treat $(AB)$ as a single block $\implies 7! = 5,040$ ways.


$$\text{Total} = 20,160 - 5,040 = 15,120$$



---

### **8. Distinct Arrangements of DATABASE**

* **Intuition:** For words with repeating letters, divide by the factorial of each repeating letter's count. For vowels together, treat the vowels as a single combined block.
* **Letter Counts:** Total = 8 letters (A appears 3 times, others appear once). Vowels = $\{A, A, A, E\}$.
* **Part A: Total Distinct Arrangements**

$$\text{Total} = \frac{8!}{3!} = \frac{40,320}{6} = 6,720$$


* **Part B: All Vowels Together**
* Group the 4 vowels into 1 block.
* Arrange $4 \text{ consonants} + 1 \text{ vowel block} = 5$ items ($5! = 120$ ways).
* Internal arrangement inside the vowel block: $\frac{4!}{3!} = 4$ ways.


$$\text{Total} = 5! \times \frac{4!}{3!} = 120 \times 4 = 480$$



---

### **9. Team Selection from Volunteers**

* **Intuition:** A team of 5 needs at least 2 coders and at least 2 testers. This gives us two valid team setups: $(2 \text{ coders, } 3 \text{ testers})$ or $(3 \text{ coders, } 2 \text{ testers})$.
* **Approach & Calculation:**
* **Case 1 (2 Coders, 3 Testers):**

$$\binom{7}{2} \times \binom{5}{3} = 21 \times 10 = 210$$


* **Case 2 (3 Coders, 2 Testers):**

$$\binom{7}{3} \times \binom{5}{2} = 35 \times 10 = 350$$




$$\text{Total} = 210 + 350 = 560$$



---

### **10. Selecting Representatives**

* **Intuition:** Asha and Neel are either both in or both out. Split into these two distinct scenarios.
* **Approach & Calculation:**
* **Case 1 (Both included):** Pick remaining 2 reps from the other 8 students:

$$\binom{8}{2} = 28$$


* **Case 2 (Both excluded):** Pick all 4 reps from the other 8 students:

$$\binom{8}{4} = 70$$




$$\text{Total} = 28 + 70 = 98$$



---

### **11. Panel Selection**

* **Intuition:** Candidate E is mandatory. For the remaining spots, use the complement rule: find all ways to pick the rest, and subtract the forbidden cases where both C and D are picked together.
* **Approach & Calculation:**
1. **Total ways to pick 4 more candidates from the remaining 10:**

$$\binom{10}{4} = 210$$


2. **Forbidden ways (both C and D included, leaving 2 spots from 8):**

$$\binom{8}{2} = 28$$




$$\text{Total} = 210 - 28 = 182$$



---

### **12. Question Paper Selection**

* **Intuition:** To pick 7 questions with at least 3 from Part I and at least 3 from Part II, the valid split combinations are $(3 \text{ from Part I, } 4 \text{ from Part II})$ or $(4 \text{ from Part I, } 3 \text{ from Part II})$.
* **Approach & Calculation:**
* **Case 1 ($3 \text{ and } 4$):**

$$\binom{5}{3} \times \binom{7}{4} = 10 \times 35 = 350$$


* **Case 2 ($4 \text{ and } 3$):**

$$\binom{5}{4} \times \binom{7}{3} = 5 \times 35 = 175$$




$$\text{Total} = 350 + 175 = 525$$



---

### **13. Grouping Volunteers**

* **Intuition:** When groups have distinct labels (tasks), group order matters. When groups are unlabeled, divide by $3!$ to remove duplicate arrangements of indistinguishable groups.
* **Approach & Calculation:**
* **Labeled Groups (Assigned to tasks):**

$$\text{Labeled} = \binom{9}{3} \times \binom{6}{3} \times \binom{3}{3} = 84 \times 20 \times 1 = 1,680$$


* **Unlabeled Groups:**

$$\text{Unlabeled} = \frac{1,680}{3!} = \frac{1,680}{6} = 280$$





---

### **14. Software Licence Installations**

* **Intuition:** First pick which 5 computers get licenses (making sure the 2 required computers are included), then assign the 5 unique licenses to those chosen computers.
* **Approach & Calculation:**
1. **Select Computers:** 2 computers are locked in. Pick the remaining 3 computers from the other 6:

$$\binom{6}{3} = 20$$


2. **Assign Licenses:** Arrange 5 distinct licenses across the 5 chosen computers:

$$5! = 120$$




$$\text{Total} = 20 \times 120 = 2,400$$



---

### **15. Robot Grid Path through Checkpoint**

* **Intuition:** Split the journey into two legs: from start to checkpoint, and from checkpoint to end. The number of paths for a leg with $\Delta x$ right steps and $\Delta y$ up steps is given by $\binom{\Delta x + \Delta y}{\Delta x}$.
* **Approach & Calculation:**
* **Leg 1: $(0,0) \to (2,1)$** (2 Right, 1 Up):

$$\binom{2+1}{1} = \binom{3}{1} = 3$$


* **Leg 2: $(2,1) \to (4,3)$** (2 Right, 2 Up):

$$\binom{2+2}{2} = \binom{4}{2} = 6$$




$$\text{Total Paths} = 3 \times 6 = 18$$



---