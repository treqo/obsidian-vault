---
tags:
  - "#textbook"
  - "#computer-science"
  - "#computers"
  - "#computer-engineering"
  - "#interpreter"
  - "#project"
  - "#coding"
time: 2024-06-22T15:36:00
status: Incomplete
current chapter: https://www.craftinginterpreters.com/the-lox-language.html
---
---

Links: [[computers]] [[projects]] [[learning]]

---

# Crafting Interpreters

Notes on the book *[Crafting Interpreters](https://www.craftinginterpreters.com/welcome.html)* by Robert Nystrom. Using C to build my own programming language.
## [Chapter 1](https://www.craftinginterpreters.com/introduction.html) – Introduction

#### Little Languages Everywhere

> *For every successful general-purpose language, there are a thousand successful niche ones.*

- These niche languages are called "little languages" or "domain-specific languages"
	e.g. Make, JSON, HTML, XML, CSS, CPP, SQL

#### The Code

**Compiler-Compilers**

Compiler-compilers – like Yacc, Lex, Bison – is a tool that takes in a grammar file and produces a source file for a compiler. In essence, it takes a custom language and outputs a compiler specific to that language.

A **Compiler** reads files in one language, translates them, and outputs them in another language. 

You can implement a compiler in any language, including the same language, in a process called **self-hosting**.

**Bootstrapping**: Even though you've written your compiler using the same language, you can't compile your compiler. So you use the technique **Bootstrapping** by getting your first compiler, compiled using another language, then building your original compiler.

> You can’t compile your compiler using itself yet, but if you have another compiler for your language written in some other language, you use _that_ one to compile your compiler once. Now you can use the compiled version of your own compiler to compile future versions of itself, and you can discard the original one compiled from the other compiler. This is called **bootstrapping**, from the image of pulling yourself up by your own bootstraps.

![[Pasted image 20240622154213.png]]
#### Challenges

1. There are at least six domain-specific languages used in the [little system I cobbled together](https://github.com/munificent/craftinginterpreters) to write and publish this book. What are they?

	Answer: Markdown, Jinja2, Makefile, SASS, CSS, HTML. There's also the homegrown little tags inserted in the code and Markdown to weave the two together.

	The tests used to ensure the interpreters work correctly also have a mini-language embedded in comments to define expectations for how the test should behave.
	
	This doesn't count the Python scripts that glue this altogether, since Python is a general-purpose language.

2. Get a “Hello, world!” program written and running in Java. Set up whatever makefiles or IDE projects you need to get it working. If you have a debugger, get comfortable with it and step through your program as it runs.

	Done. However, maybe I should use IntelliJ IDE instead of vscode.

3. 1. Do the same thing for C. To get some practice with pointers, define a [doubly linked list](https://en.wikipedia.org/wiki/Doubly_linked_list) of heap-allocated strings. Write functions to insert, find, and delete items from it. Test them.

	Done. Find the source code [here](file:////Users/tareqalansari/Desktop/Dev/proj-based-learning/c-cpp/interpreter/c-linked-list)

## Chapter 2 – A Map of the Territory

#### Introduction

> You must have a map, no matter how rough. Otherwise you wander all over the place. In _The Lord of the Rings_ I never made anyone go farther than he could on a given day. 

– J. R. R. Tolkien

---

This sort of reminds me of a [[stoicism|stoic]] quote by ancient Roman philosopher [Seneca](https://en.wikipedia.org/wiki/Seneca_the_Younger):

> If one does not know to which port one is sailing, no wind is favorable.

or in Latin

> Ignoranti quem portum petat, nullus suus ventus est.

Also, the quote by [Abraham Lincoln](https://en.wikipedia.org/wiki/Abraham_Lincoln)

> Give me six hours to chop down a tree and I will spend the first four sharpening the axe.

All these quotes speak of the value of preparation. It's easy to forget that a champion is built, hours and hours training in the shadows, not the arena.

--- 

#### The Parts of a Language

Imagine when implementing a language, that you are on a mountain. There are many different paths down, many alternatives you can take in creating this programming language. 

![[Pasted image 20240622162224.png]]

The first step, is **scanning** or **lexing**. You're given a file of text and you need to decipher it. A scanner takes in a stream of words from this file, and separates it into recognizable keywords and symbols (called **tokens**), essentially trying to make sense of it.

Then comes a **parser** which takes this flat stream of tokens, and creates organizes it into some sort of tree structure with hierarchy, with rules based on the language.

![[Pasted image 20240622162742.png]]
	*figure of a **parse tree** or **abstract syntax tree** *

Note: **syntax errors** happens when the parser recognizes we break a rule which affects the parsing of the code. Example, forgetting a `;` in C.

#### Static Analysis

Now, after we have a syntax defined, when going through the code, a process called **binding** or **resolution** occurs. We find out where identifiers are defined, and their properties (e.g. `global`, `const`, etc.), as well as scope and type. This is where **Type errors** can be thrown.

For **Statically typed** languages, type errors are thrown at **compile time**. For **dynamically typed** languages, type errors are thrown at **runtime**.

In the **AST** nodes of the tree have meta data, some of which may be left blank, and filled in incrementally when found (during **binding**). Gets stored back as **attributes**, or left off to the side in **symbol tables**.

> You can think of the compiler as a pipeline where each stage’s job is to organize the data representing the user’s code in a way that makes the next stage simpler to implement.
	– [Intermediate representations](https://www.craftinginterpreters.com/a-map-of-the-territory.html#intermediate-representations)

You don't directly convert some high level language like C, Python, Fortran into x86, ARM or MIPS. Because then you would need to make 9 compilers, one for each combination. Instead, we use **Intermediate Representation** (IR). You take the Language **front ends** and target it to another intermediate language (GCC uses GIMPLE or RTL), then from the IR you target the back end language.

Another reason for using an IR is because we can **optimize** the users code. Sometimes we can do this at compile time like the following example:

```python
pennyArea = 3.14159 * (0.75 / 2) * (0.75 / 2);
```

A process called **constant folding** occurs

```python
pennyArea = 0.4417860938;
```

Thus, no arithmetics has to be done during runtime.

Aside – Many successful languages often don't delve into compile time optimizations, rather focusing performance effort on runtime semantics.

#### Code Generation

After applying optimizations, we need to get closer to **machine code**. We don't want to convert it directly instructions usable by the CPU because computers use different architectures. So instead it is converted to **bytecode**, because each instruction is usually one byte.

From there you can either write mini compilers for each target architecture, or use a **Virtual Machine**, a program that emulates a hypothetical chip supporting your virtual architecture at runtime.

#### Compilers and Interpreters

- **Compiling** is an implementation technique that involves translating one language into another. Usually a high level language to a lower level language – lower language meaning closer to machine code. **Transpiling** to a higher level language is also a form of compiling.
- A language implementation can be a compiler, when it produces the source code into some other form. Then the user has to execute the output. However, it is an interpreter when the source code is directly executed. So it runs a program "from the source".

	GCC and Clang are compilers for C. They compile C into machine code and the user must run the executable. 

![[Pasted image 20240622231103.png]]

> Most scripting languages are interpreters, but have a compiler.

#### Challenges

1. Pick an open source implementation of a language you like. Download the source code and poke around in it. Try to find the code that implements the scanner and parser. Are they handwritten, or generated using tools like Lex and Yacc? (`.l` or `.y` files usually imply the latter.)

2. Just-in-time compilation tends to be the fastest way to implement dynamically typed languages, but not all of them use it. What reasons are there to _not_ JIT?

3. Most Lisp implementations that compile to C also contain an interpreter that lets them execute Lisp code on the fly as well. Why?

## Chapter 3 – Lox Language

#### Dynamic Typing

Lox is a **dynamically typed** programming language; meaning type errors are reported during run time (ex: trying to divide a string type). C and Java are statically typed languages. This means their type is declared during runtime, and you once it's been declared, the type can't be changed. This makes Lox similar to JS.
#### Memory Management

In high level languages, to make the code simpler for the user, and less error prone, memory is managed automatically. Two main techniques are **reference counting** and **garbage collection**. 

> Aside – There are many forms of garbage collection. This is something I learned in CPEN 212.
#### Data Types

- Boolean – `true` or `false`.
- Numbers – double precision floating point. This deals with integers and decimal numbers.
- Strings – double quotes.
- Nil – Lox's version of `null`.
#### Expressions

###### Arithmetic

- Addition, subtraction, multiply, divide

# References







