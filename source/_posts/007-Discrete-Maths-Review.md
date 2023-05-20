---
title: Discrete Maths Review
date: 2023-05-16 09:35:39
tags:
  - college
  - discrete maths
  - review
categories: Review
description: A review for Discrete Maths Final Exam.

---


<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;"> This course was previously called "Complexity and Algorithm", and we indeed studied algorithm, i still dont know what discrete maths is.  </small>

---

## Fibonacci

Four ways of computing the $n^{th}$ value in the fibonacci series. 

### Recursion

Programme using recursive function. 

Complexity = $\Theta(1.6^n)$

### Pen and paper

The idea is the same as how a human being works out the $n^{th}$ value. 

Complexity = $\Theta (n)$

We can ounly keep the last two values in order to save memory space. 

### Analytically

$$ F(n) = \frac {\phi ^n - (1-\phi ^n)} {\sqrt 5} \quad where \quad \phi = \frac {(1-\sqrt 5)} {2}$$ 

When computing large values, need more precision. May provide wrong answers when dealing with float type (caused by round up).

### Matrix multiplication

For example, $M^8 = ((M^2)^2)^2$, only takes 3 times to calculate.

Complexity = $\Theta (log (n))$

Memory = $\Theta (1)$ 

(constant memory occupation)

## Complexity notation

### Theta notation

It gives an idea of the number of steps, time of computing, amount of memory occupied, etc of an algorithm. It is both asynmptotic and worst case scenario. 

Rigorous definition: 

For $n \leq n_0$, there are constants $c_1$ and $c_2$ such that: 
$$ 0 < c_1 \leq \frac {f(n)}{g(n)} \leq c_2 $$
$$ f(n) = \Theta(g(n)) $$

### O and $\Omega$ notation

$$ 0 \leq \frac {f(n)}{g(n)} \leq c ⇔ f(n) = O(g(n)) $$

$$ \lim_{n \rightarrow \infty} \frac {f(n)}{g(n)} = 0 ⇔ f(n) = o(g(n)) $$

$$ 0 < c \leq \frac {f(n)}{g(n)} ⇔ f(n) = \Omega(g(n)) $$

$$ \lim_{n \rightarrow \infty} \frac {f(n)}{g(n)} = \infty ⇔ f(n) = \omega(g(n)) $$

### notation multiplication and addition

Constants < logarithms < polynomials < exponentials. 

### Meaning and Examples

$O(1)$: fixed amount of time. eg: look up table.

$O(log(n))$: much less than $n^{anything}$, not much more than $O(1)$. eg: binary search in sorted array. 

$O(n)$: very commonly used. eg: unsorted search, sum of array. 

$\tilde O(n) = O(nlog(n)) $: very common. eg: sorting, FFT

$O(n^2)$: convolution

$O(n^3)$: matrix multiplication (we do not want but sometimes cannot be improved)

### Travelling salesman problem

### Analysation

$$ \sum _{j=1} ^{n} f(j) = O(n^2), f(j) = O(n) $$

$$ \sum _{j=1} ^{n} f(j) = \Theta(n^2), f(j) = \Theta(n) $$


---

## Divide and conquer 

If a programme have complexity $n^2$ for 0 divides, by using divide and conquer for k divides, complexity can be improved to $O(nlog(n))$, where k is the maximum times of divide $log_2(n)$. 

### Mergesort

Naive sort: $\Theta(n^2)$

Merge sort: $O(nlog(n))$

Using joint iteration, the merge process on each level takes $O(n)$. 

### Master Method

A useful tool to calculate the new complexity after using divide and conquer. 

$$ T(n) = aT(\frac {n}{b}) + O(n^d) $$

Understanding: $T$ is the function of the original algorithm (which is not changed by divide the task into smaller parts), $a$ is the parts that have to be considered after divide, $b$ is the number of parts that the original problem is divided into, $d$ is the added complexity when merging different parts. 

$$ T(n) = \tilde O(n^{max(d, log_b a)}) $$

Or: 

$$ T(n) = O(n^d), d > log_b a $$
$$ T(n) = O(n^d log(n)), d = log_b a $$
$$ T(n) = O(n^{log_b a}), d < log_b a $$

### Loop invariant 

1. initialisation: works for the first term
2. maintenance: assume that works for k, prove that (k+1) works
3. termination: get the conclusion

### Other Uses

Maximum subarray problem

Multiplying large numbers

(Karatsuba's trick)


## Dynamic programming

Using memoisation, recursively solve complex problems by considering from the very basic cases. 

Possible approach: Top down, bottom up. 

Use of hash table. 

Procedure: 
1. Initialisation
2. recursively make the next decision


### Examples

Rod cutting problem

Longest common subwords


## Greedy algorithm

May not be the best strategy. 

### Examples

Knapsack problem

0-1 (all or none)

Fractional knapsack problem

Coin changing
- optimal substructure
- is canonical? 

Hungry students

## Graphs

### Terminology

undirected 

directed (oriented)

loops

multigraph

order = number of vertices

size = number of edges

degree of vertex = number of edges attached (indegree / outdegree)

regular: all edges have the same degree

complete: all possible edges connected

subgraph

induced subgraph: all the edges in the subgraph

weighted graphs

path

cycle

acyclic: no 

DAG = directed + acyclic

simple: no repeated vertices in path

connected: every pair joined by a path

tree

forest

Bipartite graph: 2 matching subsets

### graph data structure

#### adjacency matrix

storage cost = $\Theta(V^2)$

#### adjacency lists

storage cost = $\Theta(V+E)$

#### adjacency sets

storage cost = $\Theta(V+E)$

| Operation                   | Matrix | List | Set  |
|-----------------------------|--------|------|------|
| Add Vertex                  | O(V^2) | O(1) | O(1) |
| Remove Vertex               | O(V^2) | O(E) | O(V) |
| Add Edge                    | O(1)   | O(1) | O(1) |
| Remove Edge                 | O(1)   | O(V) | O(1) |
| Storage                     | O(V^2) | O(V+E) | O(V+E) |
| Shortest Path (Dijkstra)    | O(V^2) | O(V^2+E) | O(VlogV+E) |


### graph search

breadth-first or depth-first (both methods have complexity $O(V+E)$). 

Predecessor subgraph

### Fibonacci heap

1. "priority" for each item
2. extract highest priority item
3. increase the priority of an item

(A normal binary heap has the first two properties)

| Operation                  | Binary Heap        | Fibonacci Heap            |
|----------------------------|--------------------|---------------------------|
| Insert                     | O(log n)           | O(1)                      |
| Delete Min                 | O(log n)           | O(log n) (amortised)      |
| Decrease Key               | O(log n)           | O(1) (amortised)          |
| Merge                      | O(n)               | O(1)                      |
| Find Min                   | O(1)               | O(1)                      |
| Extract Min                | O(log n)           | O(log n) (amortised)      |
| Update Key                 | O(log n)           | O(1) (amortised)          |
| Union                      | O(n)               | O(1)                      |


### Dijkstra's algorithm

greedy algorithm: shortest weighted first. 

breadth-first

complexity: 

For list: $O(V^2+E)$

For fibonacci heap: $O(VlogV+E)$

### Topological sort

Depth-first


## Various Topics

### Amortised analysis 摊销

For some worst case analysis, it would be too pessimistic. Sometimes we say "f(n) is amortised O(g(n))". 

Example: dynamic array data strcuture. 

### Multithreaded algorithms

We use an idealised model where memory is shared, and the work is perfectly scheduled. 

#### Definitions

$T_P$ = time taken on P processors

$T_1$ = total work (when there is only one processor)

$T_{\infty}$ = span (the shortes time it would take to complete all work, using as many processors as we want)

Work law: $T_P \geq \frac {T_1} {P}$

Span law: $T_P \geq T_\infty$

Speedup = $\frac {T_1} {P} \leq P$ (perfect linear if equal)

Parallelism = $\frac {T_1} {T_\infty}$ 并行

#### Coding

- `spawn`: make new thread
- `sync`: wait for all the threads to finish

(by recursion, may acturally end up with many threads)

### Greedy scheduler

When the work is not perfectly scheduled, performance = $T_P \leq \frac {T_1} {P} + T_\infty $. 

$\frac {T_1} {P}$ is the performance when all P processors are busy, $T_\infty$ is the performance when not all the processors are used (would not improve even if there are more processors). 


## Cultural knowledge

### Intractability

NP-Hard

NP-complete

2-SAT

3-SAT

Circuit-SAT

### Computability

polinomial reduction

### Turing machines

Infinitely long tape with symbols

Finite set of internal states

Head that can moce along the tape, read and write

### Halting problem

Diophantine equations
