---
title: Software Systems Review
date: 2023-04-18 09:22:42
categories: Study
tags: 
    - study 
    - software systems
    - review
description: A review for Software Systems Final Exam. 
---

<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;">My conversation with GPT (about Hexo) eventually turned to database and network systems. So, I wrote this post to review my Software Systems course.  </small>

---

## Intro

### Overview

Almost any modern **software system** comprises of: 
- **Databases**: Where data is stored.
- **Networks**: How data move around between different computers. 
- **Applications**: How applications access and utilize networks and databases to implement their functionalities. 

An **Internet exchange point** (IXP) is a physical location through which internet infrastructure companies such as **Internet Service Providers** (ISPs) and **Content distribution networks** (CDNs) connect with each other. 

### Degrees of Data Abstraction

- **External models**
    - User’s view of the database
- **Conceptual models**
    - A logical structure of the entire database
    - Example: ER diagram
- **Internal models**
    - A representation of the database as seen by the DBMS
    - The database is considered as a collection of fixed-size records, closer to the physical level or file structure
    - Example: relational model
- **Physical models**
    - The physical implementation of the database 
    - Deal with runtime performance, storage utilization, file organization, etc.
    - Example: data in an RDBMS, such as SQLite, MySQL or Amazon RDS

### Notice

Other ways of **conceptually modeling data** than ER diagrams: hierarchical models, object-oriented models. 

Other **internal models** than the relational one: a key-store, or a document database. 

Other ways of **physically storing data** than an RDBMS: a distributed non-relational database like Amazon’s DynamoDB or MongoDB. 

---

## Entity relationship Modeling

### Primary Keys

A primary key is an attribute which uniquely identifies an 
entity in an entity set
– There must never exist two entities with the same key
– Primary keys are shown on ERD as underlined attributes
• Primary keys may be natural or surrogate
– Natural keys : attributes arising from the application data
– Surrogate key : “invented” attributes only for use within the 
database (internal functions are used to generate them)
• Primary keys may be composed of multiple attributes
– Natural keys are often composite, e.g. : 
(firstname,lastname,date_of_birth)
– We usually want the key to use the smallest possible set of 
attributes



## Relationship model


## Relationship algebra


## Layered network


## Application layer 1


## Application layer 2


## Peer to peer applications


## SQL 1 - DQL


## SQL 2 - DML & DDL


## Transport layer 1


## Transport layer 2


## Network layer 1


## Network layer 2


## Link layer 1


## Link layer 2



---

<p style="opacity: 0.7;">End Notes: 

<small style="opacity: 0.7;">This <i class="fa-sharp fa-solid fa-book"></i> [Notion site](https://amor-zhao.notion.site/Information-1dd18481598a4787b7b82b10261ea6f6) shows my logbook for the Information Processing Lab. It was not assessed, so I just used it to keep track of what I've done. Specifically, lab 5 and 6 required to use the knowledge and techniques covered in this course. </small>

