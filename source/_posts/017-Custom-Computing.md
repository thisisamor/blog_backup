---
title: Custom Computing
date: 2024-02-12 14:11:27
tags:
  - college
  - custom computing
  - review
categories: Review
description: Develop parametric descriptions of custom computers, focus on analysing, evaluating and simulating custom computers in terms of time and space. 
---

<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;">

我选这个课的原因是……？？？</small>

---

## 1. Technologies 

TPU: Tensor processing unit

Systolic array

### Key principles
Generalisation and specification 
- start with design $f0$
- generalise $f0$ into $f(x)$
- specialise $f$ with values for $x$

### Implementation 

Application-specific integrated circuit (ASIC)
- high performance, low part cost
- high risk, high development cost, slow time-to-market
- costly to develop, build and test

Field programmable gate array (FPGA)
- low risk, fast time-to-market
- post-delivery improvement

Custom computing systems

### Optimisation

Fabrication time
(pre-fab optimisation)

Compile time
(pre-execution optimisation)

Run time

## 2. Systems and Customisation 

Microprocessors: more flexible, less efficient

Dedicated hardware: more efficient, less flexible

### A chip customised for a specific application

- No instructions
- No branches
- Explicit parallelism 
- Dara streamed onto chip

### Performance estimation

### Customisation opportunities

- exploit algebraic properties
- enhance parallelism
- pipelining

## 3. Parametric Design Description

(Ruby - a declarative block description language)

### Counter-flowing data

Parallel composition

Binary relation: `x R y`

```
x inc2 y 
Rebecca: inc2 = VAR x.x $rel (`inc`(`inc`x)). 
```

### Composite data

```
<x, inc y> incLR <inc x, y> 
Rebecca: incLR = VAR x y . <x, `inc` y> $rel <`inc` x, y>. 
```

### Wire blocks (polymorphic)

```
x id x
<x, <y, z>> id <x, <y, z>>
<x, y> swap <y, x>
<x, y> π1 x
<x, x> mywire <x, y, y>    // (x, x) would be illegal
fork = VAR x . x $rel <x, x>.  
fork = x $wire <x, x>.    // wire: define wire blocks
```

Series composition

```
x (Q, R) y 
∃ s. x Q s Λ s R y
```

Parallel composition

```
<x, u> [Q, R] <y, v> 
x Q y Λ u R v
```

Abbreviations

```
fst Q = [Q, id]
snd Q = [id, Q]
```

### rbs format and Rebecca


## 4. Converse, Conjugate and Composition

### Converse

```
x R-1 y
y R x
```

### Conjugate

Q conjugated by P

```
P-1 ; Q ; P
Q \ P
```

```
P \ (Q ; R)
(p \ Q) \ R
```

### Repeated series composition

```
R^3 = R ; R ; R
```

### Repeated parallel composition

```
map_3 R = [R, R, R]
```

### Types 

Tree shaped array

Triangular arrray

## 5. Grid Components and Combinators 

### Grid components

Beside and below

```
Q <-> R
Q <|> R
fsth R = R <-> swap
```

### Grid structures

### Transposed conjugate

```
R \\ [P, Q] = [Q, P]^-1 ; R ; [P, Q]
```

### Repeated beside

## 6. Reasoning and Specialisation

Parametrisation
Inductive definition

### Pointwise proof

Algebraic laws

### Pointfree proof (by induction)

Base case
Induction case


<!-- ## 7. Sequential Design and Pipelining


## 8. Pipelining: Clustering and Slowdown


## 9. Homomorphism and Bit-level Pipelining


## 10. State Machines and beyond -->




