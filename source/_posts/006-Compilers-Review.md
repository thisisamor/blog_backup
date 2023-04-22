---
title: Compilers Review
tags:
  - study
  - compilers
  - review
tags:categories: Study
description: Review of Compilers.
date: 2023-04-22 09:03:19
---

 
<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;"> [*Compilers : principles, techniques, and tools by Aho, Alfred*](https://library-search.imperial.ac.uk/discovery/fulldisplay?docid=alma995919234401591&context=L&vid=44IMP_INST:ICL_VU1&lang=en&search_scope=MyInst_and_CI&adaptor=Local%20Search%20Engine&tab=Everything&query=any,contains,compilers&offset=0) was the textbook. It was so popular during the Easter break so I got a copy of [*Compiler design : theory, tools, and examples by Bergmann, Seth*](https://library-search.imperial.ac.uk/discovery/fulldisplay?docid=alma99775304401591&context=L&vid=44IMP_INST:ICL_VU1&lang=en&search_scope=MyInst_and_CI&adaptor=Local%20Search%20Engine&tab=Everything&query=any,contains,compilers%20design%20theory&offset=0). </small>

---

## Introduction

### Terminology

High-level languages = programming languages

Target machine: the particular machine language

Equivalent programs: always provide the same output when given the same input. 

Source program

Source language

Obeject program

Object language

Syntax: whether the input is a correctly formed program

Compiler: the output is a program

Interpreter: carries out the computations specified in the source program, the output is the source program's output

Compile time

Run time

| Compile-time errors | Run-time errors   |
| ------------------- | ----------------- |
| `a := ((b+c)*d`     | devide by 0       |
| `if x<b proc1 else proc2` | use of null pointer |

### Phases of a Compiler

*The input to a compiler is simply a string of characters.*

*In general, a compiler consists of at least three phases: **lexical analysis**, **syntax analysis**, and **code generation**.*

#### Lexical analysis (lexical scanner)

A word = lexeme = lexical item = lexical token = a finite sequence of characters. 

- Key words (eg: while, for...)
- Identifiers
- Operators (eg: +, -...)
- Numeric constants
- Character constants
- Special constants (characters used as delimeters)
- Comments

Provide **a stream of tokens** corresponding to the words, build **tables** which are used by subsequent phases of the compiler (eg: symbol table, stores all identifiers). 

Symbol table: A data structure which stores each identifier once, with the information about the identifier (kind, value, etc), usually organised as a binary search tree or hash table. 

#### Syntax analysis phase (Parser)

Provide **a stream of atoms** or **a collection of syntax trees**

A **postfix traversal** can be used to generate code from the syntax tree. (后缀遍历)

Sometimes used: **Semantic analysis**. 

#### Global optimization (optional)

Examine the sequence of atoms put out by the parser, find redundant or unnecessary instructions or inefficient code. 

(Machine independent. )

#### Code generation

Atoms or sytax trees are translated to machine language instructions, or to assembly language. 

Responsible for **register allocation**. 

#### Local optimization (optional)

Examine sequences of instructions put out by the code generator, find unnecessary or redundant instructions. 

(Machine dependent. )

### Implementation Techniques

#### Bootstrapping 

#### Cross Compiling 

#### Compiling to intermediate form

---

## Lexical Analysis

### Formal Languages

Formal language: can be specified precisely and is amenable for use with computers. 

Natual language: is normally spoken by people. 

#### Language elements

**Set**: a collection of unique items

**Empty set**: $\phi$

**String**: a list of characters from a given alphabet

**Null string**: $\varepsilon$

**Concatenation**: eg: wv is the concatenation of word w and word v. 

**Language**: a set of words

#### Regular expressions (regex)

<mark style="background-color: #9fc5e8;">L(r) = the language accepted by regex r. </mark>

<mark style="background-color: #9fc5e8;">zero</mark>: 0 is a regex. (no language)
- L(0) = $\phi$

<mark style="background-color: #9fc5e8;">unit</mark>: 1 is a regex. (empty word)
- L(1) = {$\varepsilon$}

<mark style="background-color: #9fc5e8;">singleton</mark>: if c is a single character, then c is a regex. 
- L(c) = {c}

<mark style="background-color: #9fc5e8;">alternation</mark>: if r and s are regexes, then r+s is a regex. (OR)
- L(r+s) = L(r) U L(s) 
- r + s = s + r (alternation is cummutative)
- r + (s + t) = (r + s) + t (alternation is associative)
- 0 + r = r (alternation has 0 as **identity** element)

<mark style="background-color: #9fc5e8;">concatenation</mark>: if r and s are regexes, then rs is a regex. 
- L (rs) = {w1 w2 | w1 $\epsilon$ L(r) ∩ w2 $\epsilon$ L(s) }
- is formed by concatenation of w1 and w2, where w1 is drawn from L(r) and W2 is drawn from L(s)
- r(st) = (rs)t (concatenation is associative)
- 1r = r1 = r (concatenation has 1 as **identity** element)
- 0r = r0 = 0 (concatenation has 0 as **absorbing element)

<mark style="background-color: #9fc5e8;">iteration</mark>: if r is a regex, then r* is a regex. (kleene star)
- r repeated 0 or more times. 
- (r*)* = r* (**idempotency** of iteration)

<mark style="background-color: #9fc5e8;">r+</mark>
- r repeated 1 or more times
- same as rr*

<mark style="background-color: #9fc5e8;">r?</mark>
- r repeated 0 or 1 times
- same as r + 1

<mark style="background-color: #9fc5e8;">[abcdef]</mark>
- class of characters
- same as a+b+c+d+e+f

<mark style="background-color: #9fc5e8;">[a-f]</mark>
- range of characters
- same as [abcdef]


#### Finite automation (finite state machines)

1. A finite set of states
  - one starting state (may also be an accepting state)
  - zero or more designed accepting states (double circle)

2. A state tramsition function
  - an input state
  - an input symbol
  - a result state

<mark style="background-color: #9fc5e8;">Non-Deterministic Finite Automaton (NFA)</mark>

Need to keep track of all the states you **could** be in. 

![NFA1](https://github.com/thisisamor/blog_pic/blob/main/Compilers/NFA1.jpg?raw=true)
![NFA2](https://github.com/thisisamor/blog_pic/blob/main/Compilers/NFA2.jpg?raw=true)

<mark style="background-color: #9fc5e8;">Deterministic Finite Automaton (DFA)</mark>

Need to keep track of the state you **are** in. 

Steps: 
1. Start with the starting state. 
2. "If we have input a, what are the possible states we can go to, from all of which states we are possibly in currently."
3. The "epsilon transition" ($\varepsilon$) means that we can go to either of the states. 
4. Only create a new state if there isn't one. 
5. If there is no state for a input, go to the "empty state", where it goes to itself whatever the input is. 
6. After considering all the possible states, add the final states. 
7. The final states are the ones that include any of the final states in NFA. 

### Lex

Lex is used to generate a lexical analyser. The programmer specifies the words to be put out as tokens, using a extension of regexes. Lex then generates a C function, `yylex()`, which will be the lexical analysis phase of a compiler. 

#### structure

```
// ----Definition section----
// Give options that tell lex how to run
%option noyywrap
// Import C header files as necessary
%{
  #include <studio.h>
}%
// Define names for regexes
D [ 0-9 ]
l [ A-Z ]
%%
// ----Rules section----------
// List of regexes and associated actions
// "if you see smth like..."
{L}{L}?{D} {
  printf("%s\n", yytext); 
}
// "anything else..."
. {
  printf("Error!\n);
}
%%
// ----Code section-----------
// Define any functions called in the rules section
int main() {
  yylex(); 
}
```

---

## Syntax Analysis



---

## Top down parsing





## Bottom up parsing





## Code generation





## Optimization




