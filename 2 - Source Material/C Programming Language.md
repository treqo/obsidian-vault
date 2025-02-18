**Type:** [[textbook]]  
**Topics:** #c #coding #computers
**Date:** 2024-07-28  
**Source/Author:** Brian W. Kernighan, Dennis M. Ritchie
- **Tags**
	- 

---
# Chapter 1

````ad-info
title: The `for` loop
There are three parts within for loops paranthesis that determine how the iteration works:
- The first part is the *initialization* of a variable.
- The second part is a *condition* that terminates when the condition is not met.
- The third part is an increment or decrement of a variable, which is usually the initialized variable.

```c
for(int fahr = 0; fahr <= 300; fahr += 20) {
	...
}
```
````

````ad-info
title: Symbolic Constants
Symbolic constants are useful for numbers of significant importance, or that have a meaning behind it. Writing them as a symbolic constant is useful so other readers can better comprehend your code, as well as for yourself, and perhaps so you can change the value later (good for debugging).
- *Notation*: The standard is to for symbolic constants to be written in `ALL CAPS`
- Write it in a `.h` file or at the top of a short `.c` script.

```c
#define  name  replacement_text
```
````

### Character I/O

Character input and output in C deals with text input in *streams* of characters. A *text stream* is a sequence of characters divided into *lines*; a line consists of zero or more characters followed by a newline character (`\n`).

````ad-summary
title: Character IO Functions
`getchar` returns an int *value* which represents any *ascii* character as well as any other specific values like *EOF* (End-Of-File) values.
- `EOF` is an integer value, a symbolic constant defined in C's `stdio.h`.

```c
/*
get next character or word from input stream
*/
c = getchar()

/*
output a character or word to a stream
*/
putchar(c)
```

The first iteration of a program that reads user input and outputs it, char by char, is the following.

```c
#include <stdio.h>

int main() {
	int c;
	c = getchar(); // await for user input and get first char
	
	while(c != EOF) { // while not end of file
		putchar(c);   // output the char
		getchar();    // get next char
	}
}
```

It can be simplified to

```c
#include <stdio.h>

int main() {
	int c;
	
	while( (c = getchar()) != EOF )
		putchar(c);
```
````

````ad-warning
title: Importance of Parentheses
Consider the statement from the code above:
```c
while( (c = getchar()) != EOF )
```
This evaluates the `int` variable `c` and sets it equal to the result of `getchar`. Then it compares it to `EOF`. The parentheses are necessary, and without them the statement will be equivalent to:
```c
while( c = getchar() != EOF )
// equivalent
while( c = (getchar() != EOF) )
```
so `c` will evaluate to either `0` or `1`. This is because `!=` has higher precedence than `=`.
````

- Note, EOF returns -1 as an `int` but returns `4294967295` as an unsigned int. Although I would imagine that it is indeed of type `int`.
- When writing in the terminal, on a UNIX system, note that the `EOF` signal is given through `Ctrl+D`.

````ad-summary
title: Character Counting

In this section, we explore counting characters in C. Again, we use the `getchar` method which returns an `int` value, corresponding to the next character in a given input, or `EOF` value for end-of-file. In the shell, we send an `EOF` signal with `Ctrl+D`. 

This first program uses a `long` type and a `while` loop; it continues to increment `nc` until `getchar` evaluates to `EOF`. Notice we use `%ld` because we want to output a `long`. Other combinations can be found in the unofficial C documentation, [here](https://devdocs.io/c/io/fprintf).

```c
#include <stdio.h>

int main() {
	long nc = 0;
	while(getchar() != EOF) {
		++nc;
	}
	printf("%ld\n", nc);
}
```

The second program is the same except it uses a `for` loop. The interesting thing about this is we see that the body of the `for` loop is empty, but to satisfy `C`'s grammatical rules, we use an *isolated semi-colon* `;`, which is called a **null statement**.
```c
#include <stdio.h>

int main() {
	for(double nc = 0; getchar() != EOF; ++nc)
		;
	printf("%.0f\n", nc);
}
```
````

````ad-tip
title: Variable Assignment
```C
nl = nw = nc = 0;
/* -- equivalent -- */
nl = ( nw = (nc = 0));
```
````
### Arrays

````ad-summary
title: Array DS (Data Structure)
An array in C is a Linear data structure. It organizes data of the same type contiguously in memory. It has a fixed length that cannot be changed after declaration.
```c
[type or struct] _arr_name[LENGTH];
// e.g.
int ndigits[10];
```
````

````ad-tip
Recall that using single quotes on an character has a corresponding `int` value, which can be found in the ASCII table. Thus, it is valid to do arithmetic operations with characters, as it will be interpretted as the `int` value by the machine.

```c
if(c >= '0' && c <= '9')
	++ndigit[c-'0'];
```
*for any digit character found in a sequence of characters, index the appropriate spot `c-'0'`*

![[Pasted image 20240731124709.png]]
````
#### Exercises
````ad-summary
title: Character Frequency
This was a good exercise because it shows me how I can use the ASCII table to output chars as needed.
```c
#include <stdio.h>

/* a prgoram that outputs a histogram of the frequency of different characters in a given input */
int main() {
	int char_freq[26]= {0};
	int c;

	while((c = getchar()) != EOF) {
		if(c >= 'a' && c <= 'z')
			++char_freq[c-'a'];
		else if(c >= 'A' && c <= 'Z')
			++char_freq[c-'A'];
	}

	printf("character [case insensitive] : count\n");

	for(int i = 0; i < sizeof(char_freq)/sizeof(int); ++i)
		printf("%c : %3d\n", i+97, char_freq[i]);
}
```
````

### Functions

```ad-summary
A function is useful tool that can greatly simplify code by providing a *level of abstraction*. For complex problems, we can create functions, that we can reuse throughout the program, and create smaller unit tests on the function itself. Once we know it works, we don't necessarily need to know *how* it does something, but knowing *what* it does is sufficient.

In many programming languages, the standard library includes useful functions. We don't need to know how they are implemented, we just need to know what they do, and how to properly use them.
```

`````ad-summary
title: Function Syntax
```c
[return type] _function_name([param_type] param_1, ... , [param_type] param_n) {
	...
	return _return_val // with SAME type as return
}
```
````ad-tip
title: Function Prototype
*Function prototypes* are declared before `main`, and before defining the function. The function prototype must agree with the definition and uses of the declared function.
```c
int power(int m, int n);
// Parameter names are optional for function prototype
int power(int, int);
```
````
`main` is also a function. It's return is an `int` which is formally used to indicate how the program was terminated. This is known as the [[exit status]].
- `0` : implies normal termination.
- `1` : general or unspecified error.
- `2` : misuse of shell built-ins (Bash convention)
- `127` : command not found (Bash)
- `128` : invalid argument (Bash)
- use `<stdlib.h>` macros to increase readability, for the `main` return value.
	- **`EXIT_SUCCESS`**: Typically defined as `0`.
	- **`EXIT_FAILURE`**: Typically defined as `1`.
`````
#### Exercise

`````ad-summary
title: C-to-F Conversion w/ Functions
```c
#include <stdio.h>
#include <stdlib.h>

/* Celsius to Fahrenheit conversion using a function */

void c_to_f();

void f_to_c();

void prompt();

int main() {
	int option = 1;

	printf("Please type the number for the corresponding functionality\n\n");
	while(option) {
		prompt();
		int result = scanf("%d", &option);
		if(result == 1) {
			switch(option) {
				case 0:
					break;
				case 1:
					c_to_f();
					break;
				case 2:
					f_to_c();
					break;
				default:
					printf("\n\nInput invalid, please try again.\n\n");
			}
		} else {
			printf("\n\nInput invalid, please try again.\n\n");

			// Clear the invalid input from the buffer
            int c;
            while ((c = getchar()) != '\n' && c != EOF) { }
            option = 1; // Reset option to continue the loop
		}
	}

	return EXIT_SUCCESS;
}

void prompt() {
	printf("0: exit program\n");
	printf("1: Celsius to Fahrenheit Table\n");
	printf("2: Fahrenheit to Celsius Table\n");
	printf("3: Convert Celsius value to Fahrenheit\n");
	printf("4: Convert Fahrenheit value to Celsius\n");
	printf("\n");
	printf("Option: ");
}

void c_to_f() {
	printf("\n\n");
	printf("| °C | °F |\n");
	for(int c = 0; c <= 50; c+=5) {
		printf("|%4d|%4.0f|\n", c, c*(9.0/5.0)+32.0);
	}
	printf("\n");
}

void f_to_c() {
	printf("\n\n");
	printf("| °F | °C |\n");
	for(int f = 0; f <= 200; f+=20) {
		printf("|%4d|%4.0f|\n", f, (f-32.0)*(5.0/9.0));
	}
	printf("\n");
}
```

````ad-bug
There was a *bug* in the code due to the way `scanf` handles inputs. When `scanf` encounters an invalid input – inputting a `char` instead of an integer, for example – it fails to match the format specifier, `%d`, in this case, and leaves the invalid input in the input buffer `option`. As a result, `scanf` will keep trying to read the invalid input, causing the "Invalid Input" prompt to appear indefinitely. 

To fix this, we must clear the invalid input from the buffer when `scanf` fails to read an `int`.
````

`````

### Arguments – Call By Value

````ad-summary
title: Call By Value
In C, *arguments* passed into a function are **called by value** – i.e. the called function is given the values of its arguments in temporary variables rather than the original.

This is different from *call by reference*.

```c
/* power: raise base to the n-th power; n>= 0*/
int power(int base, int n) {
	int p;
	
	for(p = 1; n > 0; --n)
		p = p * base;
	return p;
}
```

```ad-note
Recognize that the program *directly alters* the input paramater, `n`. Since C is pass by value, we know that this `n` is a copy of the input parameter, so it's okay to do this.
```
````

### Character Arrays

- [!] Do the exercises

### External Variables and Scope

```ad-summary
*local* variable declarations are made within functions, and tend to disappear after a function call. This is because the variable or pointer to the data structure, is stored within *stack* memory, and after a function is complete, its call stack goes away.

To have more permanent variables, declare a variable outside of any function and use *extern*. It is *common practice* to include `extern` variables in the **header file**. Then, infact we can omit the keyword `extern`, for any uses of that variable, if that variable is defined **BEFORE** it is used. i.e. this is why we want to write it in a header file.
```

- [!] Do the exercises

## Chapter 2 – Types, Operators, Expressions

```ad-summary
title: Data types 
- `char` : a *single byte*, capable of holding one character in the local character set.
- `int` : an *integer*, typically reflecting the natural size of an `int` on the local machine.
- `float` : single-precision floating point.
- `double` : double-precision floating point.

**Qualifiers**
- `short` and `long` (apply to `int`s)
- `signed` and `unsigned` : can be applied to `char` so it's either 0-255 or -128-127
```

### Constants

```ad-summary
terminating an int with specific letters, or other specific notation makes certain values interpretted as certain things in C
- `L`: terminating an `int` with `L` indicates it's `long`
- `U`: Unsigned `int`
- `UL`: Unsigned long int
- `1e-2` or decimal point: `double`
- `F`: `float`
- *String constant* or *string literal*: a sequence of `char` types wrapped in double quotes.
```

```c
enum boolean { NO, YES };
enum escapes { BELL = '\a', BACKSPACE = '\b', TAB = '\t',
			   NEWLINE = '\n', VTAB = '\v' };
```
- `enum`'s defines constant integer values to macros.

```c
const double e = 2.71828182845905;
const char msg[] = "warning: ";
```
The `const` qualifier can be used on arrays or variables to ensure their elements or value is not altered.


---

## Questions
- Question 1
- Question 2

## Further Exploration
- Links to related articles, videos, or books

## Personal Insights
- Personal reflections or insights gained from the media
