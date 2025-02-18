**Type:** [[course]]  
**Topics:** #math  
**Date:** 2024-08-30  
**Source/Author:** {{Source/Author}} 
- **Tags**
	- 

---
# Textbook Notes

## Section 1 – Experiments with random outcomes

```ad-summary
The purpose of **probability theory** is to build *mathematical models* of experiments with random outcomes and then *analyze* these models. 

A random outcome is *anything we can't predict with certainty*; for example, a coin flip.
```

### 1.1 Sample spaces and probabilities

````ad-note
title: $(1.1)$ Probability Model
The following components make up what we call a *probability model*.

1. The **sample space** $\Omega$, the set of all possible outcomes of an experiment. Elements of $\Omega$ are called **sample points**, denoted by $\omega$.
2. *Subsets* of $\Omega$ are called **events**. The collection of events in $\Omega$ is denoted by $\mathcal{F}$.
3. The *Probability Distribution* $P$ is a function from $\mathcal{F}$ to the real numbers. 
$$P : \mathcal{F} \rightarrow \mathbb{R}$$
Each event, $A$ has a probability $P(A)$ which satisfies the following axioms
$$
\begin{aligned}
\displaystyle
\textbf{Kolmogorov’s axioms} \\ \\
(i) \qquad & 0 \leq P(A) \leq 1  \\
(ii) \qquad & P(\Omega) = 1 \ \text{  and  } \ P(\emptyset) = 0 \\
(iii) \qquad &  \text{If } A_{1},A_{2},A_{3},... \text{is a sequence of pairwise disjoint events then}  \\ \\
\end{aligned}
$$
$$
\displaystyle
P\left( \bigcup_{i=1}^{\infty} A_i \right) = \sum_{i=1}^{\infty} P(A_i)
$$
This applies for the finite case as well.
```ad-tip
This is a consequence of setting $A_{n+1}, A_{n+2},... = \emptyset$
```

Every mathematically precise model of a random experiment or collection of experiments must be of this kind.
````

```ad-info
- The triple, $(\Omega,\mathcal{F},P)$ is called a **probability space**.
- **Pairwise Disjoint** means $A_{i} \cap A_{j} = \emptyset$. i.e. the events are *mutually exclusive*.
- axiom $(iii)$ says that probability of the union of mutually exclusive events, is the sum of their individual probabilities.
```

**Example 1.1** Identify the probability space, the sample space, a few sample points, and the collection of events in a fair coin flip.
	The sample space is $\Omega = \{H,T\}$. This means that for however many times you do this experiment, you will only ever get a heads or a tails. The collection of all events, or subsets of the sample space is $\mathcal{F}=\{\emptyset, \{H\}, \{T\}, \{H,T\}\}$. By Kolmogorov's axioms, we know that the probability for some $A_{i} \in \mathcal{F}$ is $P(A_i)$ where $0 \leq P(A_{i}) \leq 1$, and in fact, the probability of $P(\emptyset)=0$ and $P(\{H,T\})=1$. Note that $\{H,T\} \equiv \Omega$.

**Example 1.2** Rolling a standard 6-sided die.
	Sample space is $\Omega = \{1,2,3,4,5,6\}$.
	Each sample point, $\omega$, is an integer between 1 and 6.
	A possible *event* in this sample space is, $A = \{\text{the outcome is even}\} = \{2,4,6\}$. 

```ad-note
title: Ordered Pairs
Some outcomes may be described using an ordered pair. Note that in an ordered pair, order matters, so that $(i,j) \neq (j,i)$ supposing $j \neq i$.
```

### 1.2 Random Sampling

```ad-note
Sampling is choosing objects randomly from a given set
```

````ad-summary
title: Equally Likely Outcomes
Given a finite sample set $\Omega$ of size $|\Omega|$, and given some event $A$ with $|A|$, if every outcome is equally likely, then the probability, $P(A)$, is given by
$$
\displaystyle
P(A) \ = \ \bigcup_{i=1}^{|A|} P(a_i) \ = \ \frac{|A|}{|\Omega|}
$$

```ad-tip
title: Remark
Look out for phrases like:
- "an element is chosen at random"
- "chosen *uniformly* at random"

These imply that we are performing random sampling, and all outcomes are equally likely.
```
````

#### Ordered Sample

An **ordered sample** is built by choosing objects *one at a time* and by keeping track of the order in which they were chosen. Mathematically, it means we may get some ordering $(i,j,k)$ which is not the same as $(j,i,k)$, and for many reasons. For one, the order is not the same, which matters for an ordered sample. For another, $i,j,k$ could come from completely different sample spaces. Consider flipping a coin and rolling a 6-sided die. Clearly, we can never get heads from the 6-sided die.

Also note, we can have ordered sampling **without** replacement or **with** replacement. 

```ad-note
We tend to use $()$ to denote an ordered outcome and $\{\}$ for an outcome where order doesn't matter
```
#### Unordered Sample




## Lecture 1
*wed, sept 4, 2024*

## Lecture 2
*fri, sept 6, 2024*

## Lecture 3
*mon, sept 9, 2024*

## Lecture 4
*wed, sept 11, 2024*

**Example** Deal 5 cards randomly from 52 card deck (sampling). What is the  
probability of a full house?

```ad-note
title: Order Doesn't Matter
In this method we only consider the final outcome, and order doesn't matter.
```
$$
\begin{aligned}
&|\Omega| = {52 \choose 5} \\ \\
&|A_{i,j}| = {4 \choose 2}{4 \choose 3} = 6 \times 4 \\ \\
&|A| = 13 \times 12 \ |A_{i,j}| \\ \\
&P(A) = \frac{|\Omega|}{|A|}
\end{aligned}
$$
```ad-note
title: Order Matters

```

**With replacement?**

$$
\frac{4^{5} \times {5 \choose 2} \times 13 \times 12}{52^5}
$$
## HW 1

**Question 1** Let $S = \{1, \{\}, c\}$ be a sample space. List all possible events.
$$
\displaystyle
\begin{aligned}
&\Omega = S = \{1, \{\}, c\} \\
&\mathcal{F}
\end{aligned}
$$



26 , 26 , 10

62^4 - 26^4 - 26^4 - 10^4
![[Screenshot 2024-09-18 at 3.58.26 PM.png]]

$$
\displaystyle
\begin{aligned}
\mathcal{L}\bigl[100 \cos(4t) \bigr] &= \frac{s}{s^{2}+16} \\ 
\mathcal{L}\bigl[ 6 \ \Omega \bigr] &= 6 \ \Omega \\ 
\mathcal{L}\bigl[ V_L \bigr] &= sLI(s) +L \ i(0^-) \\ 
\mathcal{L}\bigl[ I_C \bigr] &= \frac{1}{sC}V_C(s) - C \ \frac{1}{ v_C(0^-)} \\ 

\end{aligned}
$$

$$
\displaystyle
\begin{aligned}
&Max\{X_{1}, X_{2}, X_{3}\} = M' \\ \\
&P[M \leq 3] = P(X_{1} \leq 3)P(X_{2} \leq 3)P(X_{3} \leq 3) = 0.5^3
\end{aligned}
$$


---
