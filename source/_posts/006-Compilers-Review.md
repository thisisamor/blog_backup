---
title: Compilers Review
date: 2023-04-22 09:03:19
categories: Review
tags:
  - college
  - compilers
  - review
description: Review of Compilers.
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

Thompson's algorithm: 

![NFA1](https://github.com/thisisamor/blog_pic/blob/main/year2/Compilers/NFA1.jpg?raw=true)
![NFA2](https://github.com/thisisamor/blog_pic/blob/main/year2/Compilers/NFA2.jpg?raw=true)

<mark style="background-color: #9fc5e8;">Deterministic Finite Automaton (DFA)</mark>

Need to keep track of the state you **are** in. 

Convert NFA to DFA: 
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

## Syntax Analysis (Parse)

*A sytax tree is a data structure in which the interior nodes represent operations, and the leaves represent operands.*

*This process is called **syntax directed translation**.*

### Grammars





### Ambiguities in Programming Languages



### The Parsing Problem










---

## Top down parsing







## Bottom up parsing





## Code generation





## Optimization



---

## Coursework 

<small style="opacity: 0.7;"> I'm reviewing this because we are having a project management viva on this project this term...</small>

We built a [C compiler](https://github.com/LangProc/langproc-2022-cw-Team09/tree/main). The source language is pre-processed C90, and the target language is RISC-V assembly. The target environment is Ubuntu 22.04, VS Code has good support for collaborative programming and working inside Docker containers. 

The following features were implemented and tested: 
- a file containing just a single function with no arguments
- variables of int type
- local variables
- arithmetic and logical expressions
- if-then-else statements
- while loops
- files containing multiple functions that call each other
- functions that take up to four parameters
- for loops
- arrays declared globally (i.e. outside of any function in your file)
- arrays declared locally (i.e. inside a function)
- reading and writing elements of an array
- recursive function calls
- the enum keyword
- switch statements
- the break and continue keywords
- variables of double, float, char, unsigned, structs, and pointer types
- calling externally-defined functions (i.e. the file being compiled declares a function, but its definition is provided in a different file that is linked in later on)
- functions that take more than four parameters
- mutually recursive function calls
- locally scoped variable declarations (e.g. a variable that is declared inside the body of a while loop, such as while(...) { int x = ...; ... })
- the typedef keyword
- the sizeof(...) function (which takes either a type or a variable)
- taking the address of a variable using the & operator
- dereferencing a pointer-variable using the * operator
- pointer arithmetic
- character literals, including escape sequences like \n
- strings (as NULL-terminated character arrays)
- declaration and use of structs

[This yacc grammar](https://www.lysator.liu.se/c/ANSI-C-grammar-y.html) was used as the basic parsing structure. With the help of a basic lexing algorithm, we worked on the corresponding ast files needed to compile C programmes. 

[This website](https://godbolt.org/) is an online compiler between different high-level languages and assembly code, and sometimes is helpful for understanding which operations should be going on. 

### Program

This is where we kept all of our pointers and type/value mappings. 

```
typedef std::map<std::string, std::string> variable_type_mapping;
typedef std::map<std::string, std::string> typedef_name_mapping;
typedef std::map<std::string, std::string> label_string_mapping;
typedef std::map<std::string, int> variable_address_mapping;
typedef std::map<std::string, int> function_address_mapping;
typedef std::map<std::string, int> variable_value_mapping;
typedef std::map<std::string, std::string> struct_type_mapping;
typedef std::map<std::string, struct_type_mapping> struct_type_tracker_mapping;
typedef std::map<std::string, int> struct_address_mapping;
typedef std::map<std::string, struct_address_mapping> struct_address_tracker_mapping;
typedef std::map<std::string, int> struct_size_mapping;
```

These represents the memory space in a computer (every time a new variable is declared it is allocated an address, although in reality the algorithm would be more complex, in order to save memory space and make processes like garbage collecting easier). There are corresponding functions to get value from the above mappings (or update, reset etc. when needed). 

```
std::vector<std::string> loop_end_label{};
std::vector<std::string> stmt_end_label{};
```
These vectors are used to keep track of the label values, for some features like `GOTO`, it is necessary to track the names of the latest labels. 

### Expression

`Expression` class provides a common base type for each step, in this way we can store and manipulate expressions of different types in the uniform structure.

```
class Expression
{
public:
    virtual ~Expression()
    {}

    virtual std::string getExprId() const
    { return ""; }

    virtual std::string getBaseReg(Program &program) const
    { return "s0"; }

    virtual std::string getType(Program &program) const
    { return "int"; }

    //! Tell and expression to print itself to the given stream
    virtual void print(std::ostream &dst) const =0;

    //! Evaluate the tree using the given mapping of variables to numbers
    virtual int evaluate(
        Program &program,
        std::ostream &w
    ) const
    { throw std::runtime_error("Not implemented."); }
};
```

Other classes in this part includes `ExpressionList` and `ArgumentExprList`. 


### Primitives

In this part we implemented three classes: `Variable`, `Number` and `StringLiteral` (all inherit from the `Expression` class). 

When evaluate functions in these classes are called, the values of the variable are mapped into the corresponding maps in `program`. 

Stack pointers are updated when needed. 

### Declaration

The `program` (as mentioned before) and output stream `w` are used for compiling the output programme. Different types of variables are declared in corresponding classes, when called by the yacc syntax tree. 

- `Declaration`
- `DeclarationList`
- `InitDeclaration`
- `InitDeclarationList`
- `TypeDeclaration`
- `IdentifierDelcaration`
- `ParameterDeclaration`
- `ConstArrayDeclaration`
- `FunctionDeclaration`
- `PointerDeclaration`
- `TypedefDeclaration`
- `StructDeclaration`

### Function

- `Function`
- `ExternalDecl`: global variable declarations and function declarations
- `TransUnit`: translate stack pointers between functions

### Operators

For each operation, trace down the syntax tree by evaluating left side and right side separately. 

Operations have different outputs for different variable types (eg: float and int )

The operators include: ADD, SUB, ASSIGN, DIVIDE, MULTIPLY, MOD, LEFT SHIFT, RIGHT SHIFT, SET LESS THAN, SET GREATER THAN, EQUAL (not / greater / less), BIT WISE / LOGICAL (AND / OR / XOR). 

### Statements

- `StmtList`
- `CompoundStmt`
- `ExpressionStmt`
- `JumpStmt`
- `GotoStmt`
- `ContinueStmt`
- `BreakStmt`
- `LabeledStmt`
- `SelectionStmt` (if else)
- `IterationStmt` (while, for)
- `SwitchStmt`
- `CaseStmt`
- `DefaultStmt`

### Enumerator

Handled initialisation and evaluation of using of enumerator. 

### Postfix

Handled different expressions using Reverse Polish Notation, implemented rules to push and pop variables and operators into and from the stack. 

### Unary

Handled functions like `size_of`, and prefix expressions. 

