# questions 

1. Suppose X, Y, Z, and W are independent random variables taking values 0 or 1 with equal
probability 1⁄2. Find P(X + Y + Z + W = 2)
2. Suppose X, Y, Z, and W are independent random variables taking values 0 or 1 with equal
probability 1⁄2. P(X = Y or Z = W)
3. Suppose X, Y, Z, and W are independent random variables taking values 0 or 1 with equal
probability 1⁄2. Find P((X + Y + Z + W = 2)|( X = Y or Z = W))
4. A delivery worker is assigned one of three types of delivery routes by drawing a card
from a box containing 6 cards numbered 1 through 6. If the card is 1 or 2, the worker gets
a downtown route. If the card is 3, 4, or 5, the worker gets a neighborhood route. If the
card is 6, the worker gets an express route. Generate the pdf for the above route selection.
5. For the problem in q4 generate expectation of the variable.
6. Calculate the variance for the problem in q4.
7. Let’s extend the q4. A downtown route normally takes 10 minutes, but with probability
1/4 traffic adds 12 extra minutes. What is Expected travel time for a Downtown route.
8. Let X and Y be discrete random variables. X takes values 0,1,2,3 and Y takes values
1,2,3,4. Their joint probability mass function is given below. Find P(X + Y ≥ 5 and Y >
X)
Y=1 Y=2 Y=3 Y=4
X=0 3/120 6/120 2/120 4/120
X=1 5/120 9/120 7/120 3/120
X=2 4/120 8/120 12/120 10/120
X=3 2/120 11/120 14/120 20/120
9. From the above table calculate Find P(X = 2 ∣ X + Y ≥ 5 and Y > X)
10. Let A, B, and C be events such that P(A) = a, P( B ∣ A ) = b, P(B ∣ A
c
) = c, P(C ∣

A ∩ B) = d, and P(C ∣ A

c ∩ B) = e. Suppose C can occur only when B occurs. Find

P(A ∩ B ∩ C)
11. Find P(A|B ∩ C) from the above data in q10.
12. Four events A, B, C, and D are mutually independent. Suppose P(A ∩ B ∩ C ∩ D) =
9/80 and P(A

c ∩ B ∩ C ∩ D) = 27/160. Let's denote P(A) = a, P(B) = b, P(C) =

c, and P(D) = d. Find a.
13. Let A, B, C, and D be events. Assume every conditional probability used below is
defined. Without making any independence assumptions, and considering chain ruke of
probability check if P(A, B, C,D) = P(D ∣ A, B, C)P(C ∣ A, B)P(A ∣ B)P(B)
14. Prove if the statement is correct “if A and B are independent and P(B) > 0, then
P(A
c
∣ B) = 1 − P(A)”
15. Assume P(A) = 0.4, P(B) = 0.5, and P(A ∪ B) = 0.7. Also, P(C) = 0.6, P(B ∩
C) = 0.25, and P(C ∣ A ∩ B) = 1/2. Find P(A ∣ B ∩ C).


# Solution


We'll solve these as a learning session, not just a list of answers. For every question, we'll identify what the problem is asking, the intuition, the concept and formula, and then the calculation. I'll also point out common mistakes and shortcuts you can use in exams.

First, one important habit: in probability, don't start calculating immediately. Start by asking:

1. What are the possible outcomes?
    
2. What event am I trying to calculate?
    
3. What information or condition am I given?
    
4. Should I count outcomes, add probabilities, multiply probabilities, or use conditional probability?
    

Let's begin with the first two questions, which use the same four random variables. Understanding these will make Question 3 much easier.

## Q1. Find \(P(X+Y+Z+W=2)\)

Given that \(X,Y,Z,W\) are independent random variables and each takes the values 0 and 1 with probability \(1/2\).

### Step 1: Understand what the variables mean

Each variable behaves like a fair coin toss:

- \(0\) represents tails.
    
- \(1\) represents heads.
    
- Each outcome has probability \(1/2\).
    

For example, if

$$  X=1,\quad Y=0,\quad Z=1,\quad W=0  $$

then

$$  X+Y+Z+W=1+0+1+0=2  $$

So we're looking for the probability that exactly two of the four variables equal 1.

### Step 2: Understand the counting approach

There are four independent binary variables. Each can take two values, so the total number of possible outcomes is

$$  N=2^4=16  $$

Because all outcomes are equally likely, each has probability

$$  P(\text{one particular outcome}) =\left(\frac12\right)^4 =\frac1{16}  $$

Now we need to count how many outcomes contain exactly two 1s and two 0s.

For example:

|\(X\)|\(Y\)|\(Z\)|\(W\)|Sum|
|---|---|---|---|---|
|1|1|0|0|2|
|1|0|1|0|2|
|1|0|0|1|2|
|0|1|1|0|2|
|0|1|0|1|2|
|0|0|1|1|2|

There are six successful outcomes.

### Step 3: Use combinations

We need to choose which two of the four positions contain a 1.

The number of ways to choose \(r\) positions from \(n\) positions is

$$  \binom nr=\frac{n!}{r!(n-r)!}  $$

Here, \(n=4\) and \(r=2\):

$$  \binom42=\frac{4!}{2!2!}=6  $$

Therefore,

$$  P(X+Y+Z+W=2) =\frac{\text{successful outcomes}}{\text{total outcomes}} =\frac6{16}  $$

Final answer

$$  \boxed{P(X+Y+Z+W=2)=\frac38}  $$

### An even faster approach: the binomial distribution

The sum of four independent Bernoulli random variables, each with success probability \(p=1/2\), follows a binomial distribution.

If \(S=X+Y+Z+W\), then

$$  P(S=k)=\binom nk p^k(1-p)^{n-k}  $$

Substituting \(n=4,\ k=2,\ p=1/2\):

$$  P(S=2)=\binom42 \left(\frac12\right)^2 \left(\frac12\right)^2 =\frac38  $$

When should you use this? Whenever a question asks for exactly \(k\) successes in \(n\) independent trials with the same probability of success.

## Q2. Find \(P(X=Y\text{ or }Z=W)\)

The four variables have the same properties as in Q1.

### Step 1: Translate the events into plain English

Define two events:

- \(A=\{X=Y\}\): the first pair has equal values.
    
- \(B=\{Z=W\}\): the second pair has equal values.
    

We're asked to find

$$  P(A\cup B)  $$

The word or means at least one of the events occurs. Both can occur simultaneously.

For two binary variables, equality occurs when both are 0 or both are 1.

|\(X\)|\(Y\)|Is \(X=Y\)?|
|---|---|---|
|0|0|Yes|
|0|1|No|
|1|0|No|
|1|1|Yes|

Thus,

$$  P(A)=\frac24=\frac12  $$

Similarly,

$$  P(B)=\frac12  $$

### Step 2: Why can't we simply add the probabilities?

A common mistake is to write

$$  P(A\cup B)=P(A)+P(B)=1  $$

This is wrong because it counts outcomes where both events happen twice.

For example, \(X=Y=0\) and \(Z=W=1\) satisfies both events.

The correct addition rule is

$$  P(A\cup B)=P(A)+P(B)-P(A\cap B)  $$

We subtract the overlap once to correct the double counting.

### Step 3: Calculate the overlap

Since the first pair and second pair are independent,

$$  P(A\cap B)=P(A)P(B) =\frac12\times\frac12=\frac14  $$

Therefore,

$$  P(A\cup B) =\frac12+\frac12-\frac14 =\frac34  $$

Final answer

$$  \boxed{P(X=Y\text{ or }Z=W)=\frac34}  $$

### Alternative intuition: use the complement

The event “\(X=Y\) or \(Z=W\)” fails only when both pairs are unequal.

Each pair is unequal with probability \(1/2\), so by independence,

$$  P(A^c\cap B^c)=\frac12\times\frac12=\frac14  $$

Hence,

$$  P(A\cup B)=1-\frac14=\frac34  $$

This complement method is often faster when the event is phrased as “at least one.”

## Q3. Find the probability distribution (PMF) of the route selection

A delivery worker draws one card from a box containing cards numbered 1 through 6.

- Cards 1 and 2: Downtown
    
- Cards 3, 4 and 5: Neighborhood
    
- Card 6: Express
    

### Step 1: Understand the random variable

A random variable assigns a value to the outcome of a random experiment.

Here, let's define \(R\) as the type of route selected. It is a categorical random variable with three possible outcomes:

$$  R\in\{D,N,E\}  $$

where \(D\) is downtown, \(N\) is neighborhood, and \(E\) is express.

Because each card is equally likely, the probability of each route is the number of cards that produce that route divided by the total number of cards.

### Step 2: Calculate the probabilities

For downtown, two cards qualify:

$$  P(R=D)=\frac26=\frac13  $$

For neighborhood, three cards qualify:

$$  P(R=N)=\frac36=\frac12  $$

For express, one card qualifies:

$$  P(R=E)=\frac16  $$

### Step 3: Write the probability mass function

For a discrete random variable, the probability mass function (PMF) specifies the probability of every possible outcome.

|Route \(R\)|Probability \(P(R)\)|
|---|---|
|Downtown|\(1/3\)|
|Neighborhood|\(1/2\)|
|Express|\(1/6\)|
|Total|1|

The probabilities must sum to 1 because these three routes cover all possible outcomes:

$$  \frac13+\frac12+\frac16=1  $$

Important distinction: For a discrete random variable, the correct term is probability mass function (PMF). A PDF usually refers to a continuous random variable.

## Q4. Find the expectation of the route-selection variable

There is a subtle point here: expectation requires numerical values. Route names alone are categorical, so we must define a numerical random variable.

For this exercise, let's use the following encoding:

- Downtown \(=1\)
    
- Neighborhood \(=2\)
    
- Express \(=3\)
    

Let \(X\) represent this numerical route code.

### Step 1: Understand expectation intuitively

The expected value is a probability-weighted average of all possible values.

It doesn't necessarily tell us which outcome is most likely. Instead, it tells us the long-run average value we'd obtain if we repeated the random experiment many times.

For a discrete random variable,

$$  E[X]=\sum_x xP(X=x)  $$

### Step 2: Substitute the values

$$  E[X] =1\left(\frac13\right) +2\left(\frac12\right) +3\left(\frac16\right)  $$

$$  E[X]=\frac13+1+\frac12  $$

Using a common denominator of 6:

$$  E[X]=\frac26+\frac66+\frac36  $$

Final answer

$$  \boxed{E[X]=\frac{11}{6}\approx1.8333}  $$

Why the encoding matters: This number is the expected route code, not the expected travel time. If the question instead defines \(X\) as the card number drawn, the expectation would be \(3.5\). Always identify exactly what the random variable represents.

## Q5. Calculate the variance of the route-selection variable

We'll continue with the same numerical encoding: downtown \(=1\), neighborhood \(=2\), express \(=3\).

### Step 1: Understand variance

Expectation tells us the average. Variance tells us how spread out the possible values are around that average.

The formula is

$$  \operatorname{Var}(X)=E[X^2]-(E[X])^2  $$

Notice that \(E[X^2]\) means the expected value of the squared random variable. It is not the same as squaring the expected value.

### Step 2: Calculate \(E[X^2]\)

Square each possible value, multiply by its probability, and add:

$$  E[X^2] =1^2\left(\frac13\right) +2^2\left(\frac12\right) +3^2\left(\frac16\right)  $$

$$  E[X^2]=\frac13+2+\frac96 =\frac{23}{6}  $$

### Step 3: Apply the variance formula

From Q4,

$$  E[X]=\frac{11}{6}  $$

Therefore,

$$  \begin{aligned} \operatorname{Var}(X) &=\frac{23}{6}-\left(\frac{11}{6}\right)^2\\ &=\frac{138}{36}-\frac{121}{36}\\ &=\frac{17}{36} \end{aligned}  $$

Final answer

$$  \boxed{\operatorname{Var}(X)=\frac{17}{36}\approx0.4722}  $$

The standard deviation, if required, would be \(\sqrt{17}/6\).

Exam tip: Calculate \(E[X]\) first, then \(E[X^2]\), and only then subtract \((E[X])^2\).

## Q6. Find the expected travel time for a downtown route

A downtown route normally takes 10 minutes. With probability \(1/4\), traffic adds 12 extra minutes.

### Step 1: Identify the possible travel times

There are two outcomes:

- No extra traffic: \(10\) minutes, probability \(3/4\).
    
- Extra traffic: \(10+12=22\) minutes, probability \(1/4\).
    

Let \(T\) denote travel time.

### Step 2: Use the expected-value formula

$$  E[T]=\sum_t tP(T=t)  $$

Substituting:

$$  E[T]=10\left(\frac34\right)+22\left(\frac14\right)  $$

$$  E[T]=7.5+5.5=13  $$

Final answer

$$  \boxed{E[T]=13\text{ minutes}}  $$

### A shortcut that builds intuition

The worker always spends 10 minutes on the normal route. The additional delay is 12 minutes, but it happens only one-quarter of the time.

So,

$$  E[T]=10+12\left(\frac14\right)=13  $$

This shortcut works because the total travel time is the constant 10 minutes plus a random additional delay.

## Q7. Find \(P(X+Y\geq5\text{ and }Y>X)\)

You are given this joint PMF:

|\(X\backslash Y\)|1|2|3|4|
|---|---|---|---|---|
|0|\(3/120\)|\(6/120\)|\(2/120\)|\(4/120\)|
|1|\(5/120\)|\(9/120\)|\(7/120\)|\(3/120\)|
|2|\(4/120\)|\(8/120\)|\(12/120\)|\(10/120\)|
|3|\(2/120\)|\(11/120\)|\(14/120\)|\(20/120\)|

### Step 1: Understand a joint PMF

A joint PMF gives the probability that \(X\) and \(Y\) simultaneously take particular values:

$$  p_{X,Y}(x,y)=P(X=x,Y=y)  $$

For example, the table tells us that

$$  P(X=2,Y=3)=\frac{12}{120}  $$

To find the probability of a more complicated event, identify all table cells that satisfy its conditions and add their probabilities.

### Step 2: Translate both conditions

We need both of these to be true:

$$  X+Y\geq5  $$

and

$$  Y>X  $$

The word and means the intersection of the two events. A cell qualifies only when both inequalities hold.

### Step 3: Find the qualifying cells

Let's inspect each row.

- When \(X=0\): \(Y>X\) holds for all four columns, but \(0+Y\geq5\) never holds because the largest \(Y\) is 4.
    
- When \(X=1\): We need \(Y\geq4\) and \(Y>1\). Only \((1,4)\) qualifies.
    
- When \(X=2\): We need \(Y\geq3\) and \(Y>2\). The qualifying cells are \((2,3)\) and \((2,4)\).
    
- When \(X=3\): We need \(Y\geq2\) and \(Y>3\). Only \((3,4)\) qualifies.
    

### Step 4: Add the probabilities

The selected cells have probabilities \(3/120,\ 12/120,\ 10/120,\ 20/120\).

$$  \begin{aligned} P(X+Y\geq5,\ Y>X) &=\frac3{120}+\frac{12}{120}\\ &\quad+\frac{10}{120}+\frac{20}{120}\\ &=\frac{45}{120} \end{aligned}  $$

Simplifying:

Final answer

$$  \boxed{P(X+Y\geq5,\ Y>X)=\frac38}  $$

Key lesson: For joint-PMF problems involving inequalities, filter the table by the conditions before adding probabilities. This avoids including cells that satisfy only one condition.

## Q8. Find \(P(X=2\mid X+Y\geq5,\ Y>X)\)

This question uses the same joint PMF from Q7.

### Step 1: Recognize conditional probability

The notation

$$  P(A\mid B)=\frac{P(A\cap B)}{P(B)}  $$

means: once we know that event \(B\) has occurred, what is the probability that event \(A\) also occurred?

Here:

- \(A=\{X=2\}\)
    
- \(B=\{X+Y\geq5,\ Y>X\}\)
    

Therefore,

$$  P(X=2\mid B)=\frac{P(X=2,\ B)}{P(B)}  $$

We already calculated \(P(B)=45/120\) in Q7.

### Step 2: Find the numerator

We need the qualifying outcomes from Q7 where \(X=2\).

Those are:

$$  (X,Y)=(2,3),\quad (2,4)  $$

Their probabilities are \(12/120\) and \(10/120\), respectively.

So,

$$  P(X=2,\ B)=\frac{12}{120}+\frac{10}{120} =\frac{22}{120}  $$

### Step 3: Divide the numerator by the denominator

$$  \begin{aligned} P(X=2\mid B) &=\frac{22/120}{45/120}\\ &=\frac{22}{45} \end{aligned}  $$

Final answer

$$  \boxed{P(X=2\mid X+Y\geq5,\ Y>X)=\frac{22}{45}}  $$

### The intuition

Imagine that you discard every outcome that doesn't satisfy the two conditions in Q7. You're left with four possible \((X,Y)\) combinations.

Out of their total probability mass \(45/120\), the combinations with \(X=2\) account for \(22/120\).

The conditional probability is therefore the fraction \(22/45\).

Common mistake: Don't divide by the total probability of the original table, which is 1. Once a condition is given, the denominator must be the probability of that condition.

## Q9. Find \(P(A\cap B\cap C)\)

Given:

$$  \begin{aligned} P(A)&=a\\ P(B\mid A)&=b\\ P(B\mid A^c)&=c\\ P(C\mid A\cap B)&=d\\ P(C\mid A^c\cap B)&=e \end{aligned}  $$

Also, \(C\) can occur only when \(B\) occurs.

### Step 1: Understand what is being asked

We want the probability that all three events \(A,B,C\) happen.

The central concept is the multiplication rule of probability:

$$  P(A\cap B)=P(B\mid A)P(A)  $$

More generally,

$$  P(A\cap B\cap C) =P(C\mid A\cap B)P(B\mid A)P(A)  $$

This is the probability chain rule.

### Step 2: Apply the chain rule

We know that:

$$  P(A)=a,\quad P(B\mid A)=b  $$

Therefore,

$$  P(A\cap B)=ab  $$

Now, given that both \(A\) and \(B\) occur, the probability of \(C\) occurring is \(d\):

$$  P(C\mid A\cap B)=d  $$

Thus,

$$  \begin{aligned} P(A\cap B\cap C) &=P(C\mid A\cap B)P(A\cap B)\\ &=d(ab) \end{aligned}  $$

Final answer

$$  \boxed{P(A\cap B\cap C)=abd}  $$

Notice that \(c\) and \(e\) aren't needed. They describe what happens when \(A\) does not occur, which isn't part of the event \(A\cap B\cap C\).

The statement that \(C\) can occur only when \(B\) occurs means \(C\subseteq B\). It isn't necessary for this particular calculation, but it becomes important in Q10–Q11.

## Q10. Use the given data to find \(P(A\mid B\cap C)\)

This is the next question in the sequence (listed as Q11 in your original sheet).

### Step 1: Write the conditional probability formula

$$  P(A\mid B\cap C) =\frac{P(A\cap B\cap C)}{P(B\cap C)}  $$

From Q9, the numerator is

$$  P(A\cap B\cap C)=abd  $$

We now need to calculate the denominator \(P(B\cap C)\).

### Step 2: Split the denominator into two cases

Whenever \(B\) occurs, exactly one of these is true:

- \(A\) occurs.
    
- \(A^c\) occurs.
    

These cases are mutually exclusive and cover all possibilities under \(B\). Consequently,

$$  \begin{aligned} P(B\cap C) ={}&P(A\cap B\cap C)\\ &+P(A^c\cap B\cap C) \end{aligned}  $$

The first term is \(abd\).

For the second term, use the chain rule:

$$  \begin{aligned} P(A^c\cap B\cap C) &=P(C\mid A^c\cap B)P(B\mid A^c)P(A^c)\\ &=e\cdot c\cdot(1-a) \end{aligned}  $$

Therefore,

$$  P(B\cap C)=abd+(1-a)ce  $$

### Step 3: Substitute into the conditional probability formula

$$  \boxed{ P(A\mid B\cap C) =\frac{abd}{abd+(1-a)ce} }  $$

This is the final answer, assuming the denominator is positive so the conditional probability is defined.

### Intuition

Knowing \(B\) and \(C\) have happened, we compare two possibilities: \(A\) happened too, or it didn't. The numerator measures the probability mass of the first possibility, while the denominator measures the combined probability mass of both.

## Q11. Find \(a=P(A)\) using mutual independence

Four events \(A,B,C,D\) are mutually independent.

Given:

$$  P(A\cap B\cap C\cap D)=\frac9{80}  $$

and

$$  P(A^c\cap B\cap C\cap D)=\frac{27}{160}  $$

We need to find \(a=P(A)\).

### Step 1: Apply mutual independence

For mutually independent events, the probability of their intersection is the product of their individual probabilities.

Let

$$  P(A)=a,\quad P(B)=b,\quad P(C)=c,\quad P(D)=d  $$

Then

$$  abcd=\frac9{80}  $$

Because \(A^c\) is also independent of \(B,C,D\),

$$  (1-a)bcd=\frac{27}{160}  $$

### Step 2: Eliminate the unknowns \(b,c,d\)

Notice that both equations contain \(bcd\). Divide the second equation by the first:

$$  \frac{(1-a)bcd}{abcd} =\frac{27/160}{9/80}  $$

Cancel \(bcd\):

$$  \frac{1-a}{a}=\frac32  $$

This cancellation is valid because the first given probability is positive, so \(abcd>0\).

### Step 3: Solve for \(a\)

$$  2(1-a)=3a  $$

$$  2-2a=3a  $$

$$  2=5a  $$

$$  a=\frac25  $$

Final answer

$$  \boxed{P(A)=a=\frac25=0.4}  $$

Exam technique: When two equations contain a common product of unknown probabilities, try dividing the equations before expanding or solving a larger system.

## Q12. Verify the probability chain rule

The question asks whether this identity is correct, without assuming independence:

$$  \begin{aligned} P(A\cap B\cap C\cap D) ={}&P(D\mid A,B,C)\\ &\times P(C\mid A,B)\\ &\times P(A\mid B)P(B) \end{aligned}  $$

Assume all the conditional probabilities used are defined.

### Step 1: Start with the left-hand side

By the definition of conditional probability,

$$  P(A\cap B)=P(A\mid B)P(B)  $$

So the final two factors on the right-hand side already give us

$$  P(A\mid B)P(B)=P(A\cap B)  $$

### Step 2: Add event \(C\)

Apply the multiplication rule again:

$$  P(A\cap B\cap C) =P(C\mid A\cap B)P(A\cap B)  $$

Substituting the previous expression,

$$  P(A\cap B\cap C) =P(C\mid A,B)P(A\mid B)P(B)  $$

### Step 3: Add event \(D\)

Once more,

$$  \begin{aligned} P(A\cap B\cap C\cap D) ={}&P(D\mid A,B,C)\\ &\times P(A\cap B\cap C) \end{aligned}  $$

Substituting the expression derived in Step 2 gives exactly the proposed identity.

Conclusion

The statement is correct. It follows from the multiplication rule of probability and requires no independence assumption.

### Why this matters

The general chain rule is

$$  \begin{aligned} P(A_1\cap A_2\cap\cdots\cap A_n) ={}&P(A_1)\\ &\times P(A_2\mid A_1)\\ &\times P(A_3\mid A_1,A_2)\\ &\times\cdots \end{aligned}  $$

You can arrange the events in different orders, as long as the conditional probabilities match that order. This is a fundamental tool for problems involving dependent events.

## Q13. Prove or disprove: If \(A\) and \(B\) are independent and \(P(B)>0\), then \(P(A^c\mid B)=1-P(A)\)

### Step 1: Understand the statement

We are given that \(A\) and \(B\) are independent. This means that knowing \(B\) occurred does not change the probability of \(A\):

$$  P(A\mid B)=P(A)  $$

The question asks whether the same principle applies to the complement \(A^c\), meaning that \(A\) does not occur.

### Step 2: Start with the definition

Conditional probability gives us

$$  P(A^c\mid B)=\frac{P(A^c\cap B)}{P(B)}  $$

Since \(A\) and \(B\) are independent,

$$  P(A\cap B)=P(A)P(B)  $$

Also, \(B\) can be split into two disjoint possibilities: \(A\) happens, or \(A^c\) happens. Therefore,

$$  P(B)=P(A\cap B)+P(A^c\cap B)  $$

Rearranging,

$$  P(A^c\cap B)=P(B)-P(A\cap B)  $$

Substitute the independence relation:

$$  P(A^c\cap B)=P(B)-P(A)P(B)  $$

Factor out \(P(B)\):

$$  P(A^c\cap B)=P(B)(1-P(A))  $$

### Step 3: Calculate the conditional probability

$$  \begin{aligned} P(A^c\mid B) &=\frac{P(B)(1-P(A))}{P(B)}\\ &=1-P(A) \end{aligned}  $$

We can cancel \(P(B)\) because the question specifies \(P(B)>0\).

Conclusion

The statement is true.

$$  \boxed{P(A^c\mid B)=1-P(A)}  $$

### An even shorter proof

Independence of \(A\) and \(B\) implies

$$  P(A\mid B)=P(A)  $$

Because \(A\) and \(A^c\) are complementary events,

$$  P(A^c\mid B)=1-P(A\mid B)  $$

Thus,

$$  P(A^c\mid B)=1-P(A)  $$

This shorter proof is usually ideal in an exam once you understand why it works.

Important insight: If an event is independent of \(B\), its complement is also independent of \(B\).

## Q14. Find \(P(A\mid B\cap C)\)

Given:

$$  \begin{aligned} P(A)&=0.4\\ P(B)&=0.5\\ P(A\cup B)&=0.7\\ P(C)&=0.6\\ P(B\cap C)&=0.25\\ P(C\mid A\cap B)&=\frac12 \end{aligned}  $$

### Step 1: Identify the required formula

We want the probability of \(A\) occurring when both \(B\) and \(C\) are known to have occurred.

By conditional probability,

$$  P(A\mid B\cap C) =\frac{P(A\cap B\cap C)}{P(B\cap C)}  $$

We already know the denominator:

$$  P(B\cap C)=0.25  $$

The only missing piece is \(P(A\cap B\cap C)\).

### Step 2: Find \(P(A\cap B)\)

Use the addition rule:

$$  P(A\cup B)=P(A)+P(B)-P(A\cap B)  $$

Rearranging,

$$  P(A\cap B)=P(A)+P(B)-P(A\cup B)  $$

Substitute the given values:

$$  P(A\cap B)=0.4+0.5-0.7=0.2  $$

### Step 3: Find the probability that all three events occur

We're told

$$  P(C\mid A\cap B)=\frac12  $$

By the multiplication rule,

$$  P(A\cap B\cap C)=P(C\mid A\cap B)P(A\cap B)  $$

Therefore,

$$  P(A\cap B\cap C)=\frac12(0.2)=0.1  $$

### Step 4: Calculate the requested conditional probability

$$  \begin{aligned} P(A\mid B\cap C) &=\frac{0.1}{0.25}\\ &=0.4 \end{aligned}  $$

Final answer

$$  \boxed{P(A\mid B\cap C)=0.4}  $$

### What is the intuition?

The condition \(B\cap C\) restricts our attention to cases in which both \(B\) and \(C\) occur. Out of the total probability mass \(0.25\) for those cases, \(0.1\) also satisfies \(A\).

Thus, the fraction that satisfies all three events is \(0.1/0.25=0.4\).

Notice that \(P(C)=0.6\) isn't needed for this calculation. Not every value in a probability question must be used.

# Final revision: the concepts you just learned

|Concept|Key formula|When to use it|
|---|---|---|
|Equally likely outcomes|\(P(A)=\frac{\text{favourable}}{\text{total}}\)|Counting outcomes|
|Binomial distribution|\(P(X=k)=\binom nk p^k(1-p)^{n-k}\)|Exactly \(k\) successes|
|Addition rule|\(P(A\cup B)=P(A)+P(B)-P(A\cap B)\)|Probability of A or B|
|Independence|\(P(A\cap B)=P(A)P(B)\)|Independent events occurring together|
|PMF|\(p_X(x)=P(X=x)\)|Distribution of a discrete random variable|
|Expectation|\(E[X]=\sum_x xP(X=x)\)|Probability-weighted average|
|Variance|\(\operatorname{Var}(X)=E[X^2]-E[X]^2\)|Spread around the mean|
|Conditional probability|\(P(A\mid B)=P(A\cap B)/P(B)\)|Probability given that B occurred|
|Chain rule|\(P(A\cap B)=P(B\mid A)P(A)\)|Combining dependent events|
|Complement|\(P(A^c)=1-P(A)\)|Probability that A does not occur|

## Test yourself before moving on

Try these without looking at the solutions above. The goal is to practise choosing the correct approach, not just memorizing formulas.

1. Two fair coins are tossed. What is the probability of getting exactly one head?

1/4

1/2

3/4

1

2. If P(A) = 0.3, P(B) = 0.4 and A, B are independent, what is P(A ∩ B)?

0.7

0.12

0.1

0.3

3. A discrete random variable takes values 0 and 1 with probabilities 0.7 and 0.3. What is its expectation?

0.3

0.5

0.7

1

4. If P(A ∩ B) = 0.2 and P(B) = 0.5, what is P(A | B)?

0.1

0.25

0.4

0.7

Reset practice

My recommendation: make sure you can solve the conditional-probability and joint-PMF questions independently before moving to more advanced probability distributions. Those two techniques appear repeatedly in statistics and machine learning.