**Type:** [[textbook]]  
**Topics:** #MATLAB 
**Date:** 2024-07-16  
**Source/Author:** Following C&A textbook
- **Tags**
	- 

---
# Notes

MATLAB is a *computing language* based on vectorial computations. It is a convenient environment for numeric and symbolic computation in signal processing, control, communications, and many other related fields.

## MATLAB for Signal Processing

The following instructions are intended to teach me anything necessary to get me started with signal processing and systems analysis in MATLAB, as a complete beginner

```ad-info
title: Script vs Function
There are two types of programs in MATLAB, a **script** and a **function**.
1. A script consists of a list of *commands* using built-in MATLAB functions, or my own functions
2. A function is a program that *can be called* by a script.

**Connections**
This is sort of like a more strict way of making modules and using them in other modules in system verilog, or defining modules in python (using classes or functions from those modules), or a header file in C.
```

```ad-note
title: Display
The MATLAB display consists of three windows
1. *Command Window* where I will type commands
2. *Command History* which keeps a list of commands that have been used
3. *Workspace* where the variables are kept

```

```ad-tip
title: Useful commands
Here are some useful commands that can be typed into the command window.

The command window accepts some `UNIX` OS default commands like `cd`, `pwd`, `ls`, etc.
- `clear all` to clear all previous *variables* and *figures* in the workspace.
- `help [function_name]` provides context on how a specific function works (built-in function)
- `edit [file_name]` to use the built-in text-editor. This command helps you write your *scripts* or *functions* directly in MATLAB. The extension for a matlab file is `.mat`
```

### Numerical Computation

numerical computation refers to numerical data content input and numerical data content output.

![[Screenshot 2024-07-16 at 1.45.15 PM.png]]

![[Screenshot 2024-07-16 at 1.59.06 PM.png]]
*Example of a MATLAB live script using built in functions*

```ad-tip
- use `;` at the end of a command to **not** output the result
- To see how a function was implemented, in the command window type `type [function]`. For example `type pol2cart` or `help pol2cart` below.

- General function structure:
	> function [output(s)] = function_name(input(s)) % function description  
	% comments  
	% author, revision
	commands
```

```
$ type pol2cart

function [x,y,z] = pol2cart(th,r,z)
%POL2CART Transform polar to Cartesian coordinates.
%   [X,Y] = POL2CART(TH,R) transforms corresponding elements of data stored
%   in polar coordinates (angle TH, radius R) to Cartesian coordinates X,Y.
%   The arrays TH and R must have compatible sizes. In the simplest cases,
%   they can be the same size or one can be a scalar. Two inputs have
%   compatible sizes if, for every dimension, the dimension sizes of the
%   inputs are either the same or one of them is 1. TH must be in radians.
%
%   [X,Y,Z] = POL2CART(TH,R,Z) transforms corresponding elements of data
%   stored in cylindrical coordinates (angle TH, radius R, height Z) to
%   Cartesian coordinates X,Y,Z. The arrays TH, R, and Z must have
%   compatible sizes.  TH must be in radians.
%
%   Class support for inputs TH,R,Z:
%      float: double, single
%
%   See also CART2SPH, CART2POL, SPH2CART.

%   L. Shure, 4-20-92.
%   Copyright 1984-2021 The MathWorks, Inc. 

x = r.*cos(th);
y = r.*sin(th);
```




---

## Questions
- Question 1
- Question 2

## Further Exploration
- Links to related articles, videos, or books

## Personal Insights
- Personal reflections or insights gained from the media
