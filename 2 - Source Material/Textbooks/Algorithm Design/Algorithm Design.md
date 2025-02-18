**Type:** [[computers]] [[3 - Links/leetcode]] [[algorithms]]  
**Topics:** #  
**Date:** 2025-01-18  
**Source/Author:** {{Source/Author}} 
- **Tags**
	- 

---

# Chapter 1 – Introduction

## 1.1 – Stable Matching

## 1.2 – Five Representative Problems

### Interval Scheduling Problem

````ad-summary
title: Bipartite graph
![[Pasted image 20250205084405.png|300]]
A **Bipartite graph** is a graph in which the nodes can be divided into two **independent sets** $U$ and $V$.

Recall that an **Independent set** mean that $u_{1}, u_{2} \in U$, there exists no edge between them, or $(u_{1},u_{2}) \notin G[E]$.

```ad-note
title: Independent Set of size $k$ problem
Consider for some graph $G(V,E)$, that we would like to know if it satisfies having an independent set of size $k$. The brute force algorithm would enumerate all subsets of $k$ nodes and for each subset, it would check whether there is an edge joining any two members of $S$.

The fact that we are enumerating over all possible subsets of size $k$ means we have $\displaystyle{n \choose k}$ $\leq \displaystyle{\frac{n^k}{k!}}$. Thus, we have an $\displaystyle{O(n^k)}$ algorithm.
```

````

# Chapter 2

## 2.5 – Priority Queues



---

## Questions
- Question 1
- Question 2

## Further Exploration
- Links to related articles, videos, or books

## Personal Insights
- Personal reflections or insights gained from the media