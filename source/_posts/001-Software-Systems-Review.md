---
title: Software Systems Review
date: 2023-04-18 09:22:42
categories: Review
tags: 
    - study 
    - software systems
    - review
description: Review notes of Software Systems. 
---

<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;">My conversation with GPT (about Hexo) eventually turned to database and network systems. So, I wrote this post to review of my Software Systems course. (It's the 2nd last exam on the timetable so I wasn't expecting to post this one first.) </small>

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
    - eg: <mark style="background-color: #9fc5e8;">ER diagram</mark>
- **Internal models**
    - A representation of the database as seen by the DBMS
    - The database is considered as a collection of fixed-size records, closer to the physical level or file structure
    - eg: <mark style="background-color: #9fc5e8;">Relational model</mark>
- **Physical models**
    - The physical implementation of the database 
    - Deal with runtime performance, storage utilization, file organization, etc.
    - eg: <mark style="background-color: #9fc5e8;">data in an RDBMS</mark>, such as SQLite, MySQL or Amazon RDS

### Notice

Other ways of **conceptually modeling data** than ER diagrams: hierarchical models, object-oriented models. 

Other **internal models** than the relational one: a key-store, or a document database. 

Other ways of **physically storing data** than an RDBMS: a distributed non-relational database like Amazon’s DynamoDB or MongoDB. 


---

## Entity Relationship Modelling (ERM)

### Conceptual model of data

- An abstract structural representation of a system’s data
    - Do not identify the data structures and storage optimizations. 
    - Simply try to capture a complete synthesis of the data and the operations to implement upon it. 

- All the high-level functions of a system should be captured by a conceptual model
    - eg: searching Netflix movies by genre, by directors, by collaboration, etc.

### Entity Relationship Modelling

An Entity Relationship Model is a conceptual model of data that is about identifying the entities which make up the data and the relationships between these entities

Components of an ER Model: 
- **Entities**: the “things” in the system
- **Relationships**: how the entities relate to each other (and themselves)
- **Attributes**: properties of an entity or relationship

Components of an ER diagram: 
- **Entity Sets**
    - A set of distinguishable entities that all have the same set of properties (attributes). 
    - Could be physical things, events, conceptual...
    - Normally correspond to nouns 
    - Rectangle
- **Relationship Sets**
    - A relationship set describes how two or more entity sets are related to each other. 
    - Usually correspond to verbs : owns, has, drives...
    - Entity sets can be involved in many relationship sets 
    - Diamond
- **Attributes**
    - Properties or attributes of an entity or relationship set. Underlined attributes are primary keys, which uniquely identify entities. 
    - Small circles


### Primary Keys

A primary key is an attribute which **uniquely identifies an entity** in an entity set
- There must never exist two entities with the same key
- Primary keys are shown on ERD as **underlined attributes**

Primary keys may be natural or surrogate
- **Natural keys** 自然键: attributes arising from the application data
- **Surrogate keys** 代理键: “invented” attributes only for use within the database (internal functions are used to generate them)

Primary keys may be composed of multiple attributes
- Natural keys are often composite, eg: (firstname, lastname, date_of_birth)
- We usually want the key to use the smallest possible set of attributes

### Complex attributes

Some more complex types of attributes: 

- **Computed attributes**: properties that can be calculated from other attributes
    - Sometimes called **derived attributes**
- **Multi-valued attributes**: sets of multiple values
- **Composite attributes**: logical properties that break down into sub-attributes

### Cardinality and Participation constraints

Constrain relationships: 
- to model how the world actually works
- **to eliminate the possibility of nonsensical relations**

**Cardinality constraints**: How many times does each entity appear?
- One-to-one: “each person has exactly one ID card”
- One-to-many: “each person can manage multiple computers”
- Many-to-many: “multiple students can register in modules”

**Participation constraints**: Are entities required to appear in the relationship?
- Each person must have a swipe card
- Each module must have a teacher

### Self-relationships
Some binary relationships can involve the same entity set twice. 
- “A movie may be a sequel of another movie”
- “A studio owns another studio”
- We need to label the two connecting lines to distinguish the roles played by them.

### Relationship set attributes
Some relationship sets might have their own attributes
- “A sequel has a part number” (as measured from the first movie)

Must make sure these are really on the relationship sets, not one of the entity sets

### Ternary relationships
Some relationships involve more than two entity sets. 

#### Ternary relationships

Relationships involving three entity sets are called ternary relationships. 

- Ternary relationships are less common than binary relationships, but they can sometimes simplify the conceptual model. 
- eg: “Studios may promote movies on Netflix featuring their actors”
    - This represents a high level function in Netflix that allows a studio to promote a movie using its actors in the promotional material.

#### Weak entity

- Ternary relationships are a more accurate representation of the real world scenario, however, they break into binary relationships internally. 
- We can re-draw ternary relationships as a set of binary relations, using a weak entity
    - A **weak entity** is one that does not have its own primary key

Weak entities do not have their own stand-alone primary keys. In the internal model, they acquire keys from the entities participating in relationships with them, getting a borrowed composite primary key as a result. This makes sense because the only reason why weak entities exist are their relationships with other other entities, unlike strong entities which are representations of actual things in the external data, such as, in our example, Actor, Movie and Studio, etc

### Inheritance 

Attributes that are shared by several entities
- eg: Actor, Director and User all have names: first name and second name, gender and age. 

Inheritance was not originally a part of ER diagrams. But later it was added into the Extended ER diagrams. In the original ER diagrams, an inheritance relationship is modeled as a binary ‘is a’ relationship with full participation on the child side and a 1-1 cardinality. 

### Completed ER diagram

Below shows a short and simple version of Netflix movie meta-data: 

![ER diagram](https://github.com/thisisamor/blog_pic/blob/main/year2/Software_Systems/ER-diagram.jpg?raw=true)

*How to draw it?* 

1. Decide Initial entity sets 
    - rectangle
    - eg: Movie, Studio, Actor
2. Add attributes to entity sets 
    - circle
    - eg: title, sname, release_date, etc.
3. Add basic relationship sets 
    - diamond
    - eg: stars in, publishes
4. Underline primary keys for each entity set
    - eg: mid, sname, aid
5. Add computed attributes 
    - dashed line and circle
    - eg: isNewRelease
6. Add multi-valued attributes 
    - concentric circle
    - eg: contact_no
7. Break down composite attributes
    - circle
    - eg: fname, lname
8. Add participation constraints
    - double line
    - eg: `Movie==stars in` (every movie must be stared in by an actor)
9. Add cardinality constraints
    - numbered square
    - eg: `publishes [N] Movie` (a studio may publish any number of movies)
10. Add self-relationships
    - diamond
    - add constraints if needed
    - eg: sequel of
11. Add relation attributes
    - circle
    - eg: partno
12. Add ternary relationships
    - diamond
    - eg: promotion could be shown as a ternary relationship
13. Add weak entity
    - concentic rectangle
    - eg: promotion
14. Add inheritance
    - rectangle, diamond, circle
    - create a parent entity set from which other entity sets inherit
    - add constraints if needed
    - eg: create `Person`, Actor is a Person, Director is a Person

---

## Relationship model (RM)

### Database based on  the relationship model
- All the data is represented in terms of <mark style="background-color: #9fc5e8;">tuples (rows)</mark> in <mark style="background-color: #9fc5e8;">relations (tables)</mark>. 
- Relationships between data are implemented by **joining together different tables** based on the attributes **common** among them. 
- Further filters can then be applied to this joined tables to produce specific information. 

### Basic components

#### Field (attribute, column)
- Each field has a **unique** name
- Each field stores a **single** item of data
When implemented as a physical model
- Each field has a **particular** data type (eg: text, boolean, etc)
- Each field can have its own **validation rules** (to represent constraints, eg: can't be null)

#### Table (relations)
- Each table must have one or more fields which **uniquely identify a record in the table**, which is called the <mark style="background-color: #9fc5e8;">key field</mark> or the <mark style="background-color: #9fc5e8;">primary key</mark>. 

- Two tables may be **joined** through their key fields. 
    - The key field of one table must appear as a <mark style="background-color: #9fc5e8;">foreign key</mark> in the other table. 
    - The ability to create joins makes the relational model a powerful and flexible representation of data. 
    - **Referential Integrity** 参照完整性: When a primary key appear as a foreign key in other table, referential integrity is the logical dependency of a foreign key on a primary key. It ensures that foreign keys always have a valid reference to an existing row in the referenced table. 

#### Keys
- If a relation has more than one key, each is called a <mark style="background-color: #9fc5e8;">candidate key</mark>. 
    - One of the candidate keys is arbitratily designated to be the <mark style="background-color: #9fc5e8;">primary key</mark>. 
    - The rest are <mark style="background-color: #9fc5e8;">secondary keys</mark>. 

- If an attribute is a member of some candidate key of R, it is called a <mark style="background-color: #9fc5e8;">prime attribute</mark> of R
- Attributes that are not prime are called <mark style="background-color: #9fc5e8;">non-prime attributes</mark>.

### Relational Schema

- The name of a relation and its set of attributes together
- eg: Movies (title,  year, length, genre)
- The schemas of the **entire set of relations** of a database make up the <mark style="background-color: #9fc5e8;">relational database schema</mark> (or database schema). 

![Relational Schema](https://github.com/thisisamor/blog_pic/blob/main/year2/Software_Systems/relational%20schema.jpg?raw=true)

*How to convert the conceptual model (ER diagram) to an internal model (relational schema)?*

1. strong entity sets -> relations
    - eg: Movie(), Actor(), Studio()
2. weak entity sets -> relations
    - borrow keys from all entities
    - Promotion()
3. 1-to-many realtionships
    - migrate the key from 1-side to many-side
    - eg: Movie (sname)
4. 1-to-many with partial participation
    - create new relations
    - migrate keys from both sides
    - eg: Owns()
5. many-to-many relationships
    - create new relations
    - migrate keys from both sides
    - eg: sequel_of(), stars_in()
6. relationship attributes
    - add to appropriate relations
    - eg: sequel_of (partno)
7. multi-valued attributes
    - create new relations
    - eg: contact()

### Anomaly and Redundancies

#### Insertion anomaly
- Values of attributes not known at the insertion time and need to be assigned NULL values

#### Deletion anomaly
- Deleting one part of the information deletes another

#### Updation anomaly
- Updating the value of one of the attributes of a particular department, must update the tuples of all the employees who work in that department. 

### A good relational schema
- Do not combine attributes from multiple entity types and relationship types into a single relation. 
- Design the relational schema so that no insertion deletion or updation anomalies are present in the relations. 
- Avoid placing attributes in a relation whose values may frequently change. 
- Minimise redundant copying of attribute values in relations. 

### Normalization and Functional Dependencies

Normalization of data can be considered a process of analysing the given relational schema based on their Functional Dependencies (FDs) and primary keys to: 
- minimize redundancy 
- minimize insertion, deletion, and update anomalies. 

#### Functional Dependencies
- for any two tuples t1 and t2 in R, if t1[X] = t2[X], then t1[Y] = t2[Y]
- given the value of attributes in X, can uniquely determine the values of attributes in Y
- denoted by X → Y

##### Full functional dependency 
- If removal of any attribute A from X means that the dependency does not hold any more. 
- eg: {Ssn, Pnumber} -> Hours

##### Partial functional dependency
- If after removal of some attributes the denpendency still holds
- eg: {Ssn, Pnumber} -> Ename

##### Transitive dependency
- The functional dependency X → Y in a relation R is a transitive dependency if there is a set of attributes Z that is neither a candidate key nor a subset of any key of R, and both X → Z and Z → Y hold.

*The following are four forms of relations which are at increasing stages of refinement (1st, 2nd, 3rd, BCNF)*

#### First normal form (1NF)
- A relation has no composite attributes or multivalued attributes. 
- In other words, each attribute value in the table must be atomic. 

#### Second normal form (2NF)
- The relation is in 1NF. 
- Every nonprime attribute in R is **fully Functionally dependent** on the **primary key** of R. 
- 1NF to 2NF: 
    - Identify all functional dependencies whose left sides (determinants) are proper subsets of the primary key, and right sides are non-prime attributes. 
    - Break the table accordingly. 
    - The splitted tables are 2NF

#### Third normal form (3NF)
- The relation is in 2NF. 
- No nonprime attribute in the relation is transitively dependent on the primary key. 
- If there are nonprime attributes Z, Y in the relation, such that, for the primary key X, X → Z and Z → Y
- 2NF to 3NF: 
    - Split the relation on Z
    - Repeat the process on the resultant tables 

#### Boyce-Codd Normal form (BCNF)
- The relation is in 3NF. 
- All determinants (left hand sides of dependencies) are candidate keys, i.e. possible primary keys. We want to remove redundancies within the prime attributes themselves
- If there is a non-prime attribute Z, and a prime attribute X, such that Z → X exists
- eg: {Student, Course} → Instructor, Instructor → Course
- 3NF to BCNF:
    - Split the table on Z.
    - Repeat the process on the resultant tables 

---

## Relationship algebra

A set of operators to **combine and filter** the information in various tables, to implement high-level functions of the system. 

The set of mathematical operators used are called <mark style="background-color: #9fc5e8;">relational algebra operators</mark>. 

Relational algebra statements are like ‘intermediate code’ for SQL statements, which constitute a higher-level language.


### Relational algebra operators

| Set Operators | Database Operators |
|---------------|--------------------|
| <ul><li>Union</li><li>Intersection</li><li>Difference</li><li>Cartesian Product</li><ul> | <ul><li>Select</li><li>Project</li><li>Join</li><li>Aggregate</li><li>Division</li><li>Rename</li><ul> |

#### Union
- $(r + s)$ or $(r U s)$
- Relations must be union compatible
- Duplicate rows are eliminated

#### Intersection
- $(r ∩ s)$
- Relations must be intersection compatible

#### Difference
- $(r - s)$
- Relations have to be difference compatible
- Includes rows that are in r but not in s

#### Projection
- ( ${\Pi}_{COND} (r) $ )
- Extracts certain columns from the table

#### Selection
- ( $ {\sigma}_{COND} (r) $ )
- Extracts certain rows from the table and discards the others. Retrieved tuples must satisfy a given filtering condition.

#### Cross product (cross join)
- $(r X s)$
- Concatenates rows from two relations, making all possible combinations of rows.

#### Join (Inner join)
- ($r ⋈_{COND} s$)
- Combine related tuples from two relations.
- COND = the matching condition

#### Natual join
- ($r * s$)
- Combines tuples of two relations using an implicit condition, i.e. the tables are related by columns that have the same names and domains.
 
#### Left natural join
- ($r ⟕_{COND} s$)
- Combines tuples of two relations and keeps in the result every tuple from the left table, but only those from the right table that meet the join condition.

#### Aggregation
- ($_{Grouping attribute} F _{function list} (relation name)$ ) 
- The function list contains simple mathematical functions, eg: MAX, MIN, AVG, SUM, COUNT
- A column name may be given as a grouping attribute to fragment the function outputs into groups. 

#### Rename
- $ρ (c1, c2, …, ck) (E) $
- Rename the columns of E to c1, c2, …, ck respectively.   


### RDBMS

#### Introduction

- A database is a structured collection of data that is managed by a DBMS (Database management system)
	
- Some of the most powerful and widely used database management systems are relational: RDBMS.
They provide a high-level query language called SQL, which is based on relational algebra. 
Examples of RDBMS: Oracle, MySQL, Microsoft SQL Server, PostgreSQL, SQLite, Amazon RDS, etc. 



#### RDBMS vs. Flat Files  

- File systems do not provide a guarantee against loss of data unless backed up explicitly.
- Efficient access to specific content in a files is difficult -- not explicitly provided.
- File systems do not provide an associated high-level language for the definition, manipulation and querying of data.
- Do not provide mechanisms to control concurrent access to a file and ensuring the consistency of data and atomicity of operations.
- A relational database management system, or RDBMS, handles these issues particularly well.     


#### ACID properties of RDBMS

- Atomicity: transactions, which consist of multiple statements, are treated as atomic.
Either the whole thing succeeds or the whole thing fails

- Consistency: transactions can only bring the database from one valid state to another
a state is valid if it follows all the specified rules including constraints

- Isolation: ensures that concurrent execution of transactions leaves the database in the same state that would have been obtained if the transactions were executed sequentially. 

- Durability: database commits are final and durable even if system fails after a commit. 

---

## SQL - Data Query Language (DQL)







---

## SQL - Data Manipulation Language (DML)


---

## SQL - Data Definition Language (DDL)



---

<small style="opacity: 0.7;">Part two is about the layered network, I will finish this later. </small>


## Layered network


## Application layer


## Peer to peer applications



## Transport layer


## Network layer


## Link layer 



---

<p style="opacity: 0.7;">End Notes: 

<small style="opacity: 0.7;">This <i class="fa-sharp fa-solid fa-book"></i> [Notion site](https://amor-zhao.notion.site/Information-Processing-Logbook-a2f60455afb44e84a3b1b1c15cbb5d14) shows my logbook for the Information Processing Lab. It was not assessed, so I just used it to keep track of what I've done. Specifically, lab 5 and 6 required to use the knowledge and techniques covered in this course. </small>

