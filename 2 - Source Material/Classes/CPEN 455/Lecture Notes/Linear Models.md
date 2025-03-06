**Course:** [[CPEN 455 – Deep Learning]]
**Subject**: #
**Date:** 2025-03-02  
**Instructor:** {{Instructor}}

---
# Notes

## Table of Contents

1. [[#Statistical Learning Setup]]
2. [[#Linear Regression]]
	- [[#Problem Specification]]
	- [[#Model Design]]
	- [[#Loss Design]]
	- [[#Inference Algorithm]]
	- [[#Learning/Training Algorithm]]
	- [[#Validation and Testing]]
3. [[#Linear Classification]]
	- [[#Logistic Regression]]
	- [[#Multiclass Logistic Regression]]

## Statistical Learning Setup

````ad-summary
title: Statistical Learning Setup
Statistical Learning Setup refers to how we go from data to a trained model.

![[Screenshot 2025-03-02 at 9.44.53 AM.png|500]]
$\textit{Dataset} \rightarrow \textit{Hypothesis Class} \rightarrow \textit{Learning Algorithm} \rightarrow \textit{"best" model}$

Key Features:

**i.i.d. Dataset** $D$

An i.i.d. dataset refers to a set of data, which contains some number of samples which can be various representations. For example, the input could be a single number, a vector, matrix, etc. In the image above, we see the data set is an ordered pair $(X,y) \in D$ such that $X$ is an RGB image, (thus it is a tensor of some size), and $y$ is a string specifically a label of the image.

```ad-info
title: i.i.d.
i.i.d. is an acronym for independently identically distributed. Within the context of deep learning and datasets, this means that a sample is pulled from the same probability distribution and is sampled independently (i.e. other samples don't affect the likelihood that it is picked).
```

- [[Hypothesis Class]] $\mathcal{H}$:

This represents the set of all possible functions or models that the trained model can take (or that you're willing to consider). 

- [[Learning Algorithm]]

The learning algorithms job is to search through the hypothesis class and select the best possible model. The best possible model is usually determined by doing some measurable optimization, ex: minimizing loss on a specified loss function.  

- "best model"

The best model is ideally one that generalizes well: this means its performance on the training data is similar to that of its performance to the test data. 
````

**(1) Assumptions of IID Sampling and unknown data distribution**

Training data are *sampled* from an unknown data distribution in an i.d.d. fashion. Consists of an *input* and an *output*.

```ad-warning
title: Input and Output
Either input or output could be continous, discrete scalars, vectors, tensors, sets, graphs, etc.

E.g.
$$
\begin{aligned}
\text{Regression} \quad &x_{n} \in \mathbb{R}^{2} \quad y_{n}\in\mathbb{R} \\
\text{Classification} \quad &x_{n} \in \mathbb{R}^{2} \quad y_{n}\in \{1,2,\cdots,K\}
\end{aligned}
$$
```

$$
\displaystyle
(x_n,y_{n}) \stackrel{\mathrm{iid}}{\sim} \mathbb{P}_{\text{data}}(x,y) \quad n=1, \cdots, N
$$
*The squiggle ($\sim$) means "Is Distributed As" in probability mathematics*

Therefore, the training dataset

$$
D = \{(x_{n}, y_{n}) \ | \ n=1,\cdots,N\} \sim \mathbb{P}_{\text{data}}(x,y)^N
$$
i.e. consists of $N$ such points from the data distribution.

**(2) Model and Loss**

We introduce a *model* – i.e. [[hypothesis]] – $f(x,w)$ with learnable parameters $w$.

````ad-note
title: Hyperparameters
- [[hyperparameter|hyperparameters]] are **fixed** and **not learnable**.
- They belong to a hypothesis class $f(x,w) \in \mathcal{H}$
```ad-tip
title: Example
$$
\mathcal{H} = \{f(x,w) \ | \ f(x,w) = w^{\top}x, \ ||w|| \leq 1\}
$$
*A hypothesis class of all linear models with weight norm no larger than 1*
```
````

````ad-note
title: Loss
[[loss function|Loss]] is denoted as $\ell(y,f(x,w))$
- [[Generalization Error]] – also known as *risk* or *expected loss*
$$
\displaystyle
\mathbb{E}_{\mathbb{P}_{\text{data}}(x,y)}\bigl[ \ \ell(y,f(x,w)) \ \bigr]
$$

- [[Training Error]] – also known as *empirical risk* or *training loss*
$$
\displaystyle
\frac{1}{N} \sum_{n=1}^{N} \ \ell\bigl(y_n,f(x_n,w)\bigr)
$$
```ad-tip
title: Monte Carlo Estimation
The basic idea of a Monte Carlo estimator is to approximate an expectation by an average over _i.i.d._ samples. Specifically, if we want to compute
$$
\mathbb{E} \big[ \ell(y, f(x,w)) \big]
$$
but can't evaluate it analytically (this is what happens in the real world), we instead draw $N$ independent samples $(x_n, y_n)$ from the underlying distribution $\mathbb{P}_{\text{data}}$​. Then we approximate the expectation by the _empirical average_:
$$
\displaystyle
\frac{1}{N} \sum_{n=1}^{N} \ \ell\bigl(y_n,f(x_n,w)\bigr)
$$
By the [[Law of Large Numbers]], this sample average converges to the true expectation as $N$ grows large.
```

````

**(3) **

## Linear Regression

### Problem Specification

### Model Design

### Loss Design

### Inference Algorithm

### Learning/Training Algorithm

### Validation and Testing 

## Linear Classification

### Logistic Regression

### Multiclass Logistic Regression

---
## Questions
- 
## Practice Problems 
2. Problem 1
	- Solution
## Additional Resources
- 
