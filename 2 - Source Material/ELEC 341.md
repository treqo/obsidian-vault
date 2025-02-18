**Type:** [[]]
**Topics:** #  
**Date:** 2024-12-27  
**Source/Author:** {{Source/Author}} 
- **Tags**
	- 

---
# Notes

## Section 2.2 – The Basics

```ad-note
title: CPP is a compiled language
**CPP is a compiled language**. This means it gets compiled directly to assembly. Source files get compiled into object files, which gets linked and turned into a single executable.

![[Screenshot 2024-12-27 at 8.47.17 PM.png]]
```
### 2.1

````ad-info
title: Operator
`<<` is an operator which writes the second argument to the first.
```cpp
// [arg 1] << [arg 2];
std::cout << "Hello, World!\n";
```
````

![[Screenshot 2025-01-07 at 10.42.10 AM.png]]
*use the `sizeof` function to determine how many bytes a type is*

```cpp
void increment() { 
	int v[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}; 
	
	for (auto& x : v) ++x; // auto &x uses the actual elements, not a copy of the element
	
	for (auto x : v) std::cout << x << " "; 
	// result is {1,2,3...}
}
```

---

## Questions
- Question 1
- Question 2

## Further Exploration
- Links to related articles, videos, or books

## Personal Insights
- Personal reflections or insights gained from the media





