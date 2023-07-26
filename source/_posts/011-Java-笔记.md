---
title: Java Notes
date: 2023-07-16 10:20:17
categories: Intern
tags:
  - intern
description: 
---
<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;"> </small>

---

## Introduction

In Java, every line of code that can actually run needs to be inside a class. 

```
public class Main {
    public static void main(String[] args) {
        System.out.println("This will be printed");
    }
}
```

- `public` means that anyone can access it.
- `static` means that you can run this method without creating an instance of Main.
- `void` means that this method doesn't return any value.
- `main` is the name of the method. 

---

## Variables and Types

[tutorial website](https://www.learnjavaonline.org/)


```
String a = new String("Wow");
String b = new String("Wow");
String sameA = a;

boolean r1 = a == b;      // This is false, since a and b are not the same object
boolean r2 = a.equals(b); // This is true, since a and b are logically equals
boolean r3 = a == sameA;  // This is true, since a and sameA are really the same object
```


## Array

Arrays in Java are also objects. 

