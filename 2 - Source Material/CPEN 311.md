**Type:** [[course]]  
**Topics:** #  
**Date:** 2024-08-30  
**Source/Author:** {{Source/Author}} 
- **Tags**
	- 

---

## Git and GitHub Fundamentals

```ad-summary
`git` is a **distributed Version Control System (VCS)**, which means it's a useful tool for easily tracking changes to your code, collaborating and sharing.
```

## Lecture 1 - Synthesizable Verilog

```ad-summary
title: 3 Pattern Rules for Synthesizable verilog
1. Purely Combinational
2. Sequential
```

Each process/always block gets converted to a piece of hardware.

an always block models the behavior of a block of hardware

Bit width mismatch

```v
wire [7:0] bus;
wire [15:0] longer_bus;

assign bus = longer_bus // Implicitly bus[7:0] = longer_bus[7:0]

assign longer_bus = bus // longer_bus[7:0] = bus[7:0]
```

uses least significant bits.

adding two numbers results in the max bit width of the two numbers.

S

---
