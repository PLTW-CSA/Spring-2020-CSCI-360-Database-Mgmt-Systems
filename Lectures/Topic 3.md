# CSCI 360 Spring 2020
# Dr. Ning Zhang

# Topic 3: The Entity-Relationship Model

## Outline
+ Concept
+ Characteristics
+ Constraint
  - Key Constraints
  - Entity Integrity
  - Referential Integrity
  - Semantic Integrity Constraints
+ Relational Schema Diagram
+ Operations on Relation 
  - Concepts
  - Constraint Violation
  
## Objectives
+ Relational Model Concepts 
+ Relational Model Constraints 
+ Update Operations
+ Dealing with Constraint Violations


## Concept
### History of Relational Model
+ A Relation is a mathematical concept based on the ideas of sets
+ The model was first proposed by Dr. E.F. Codd of IBM Research in 1970 in the following paper: “A Relational Model for Large Shared Data Banks,” Communications of the ACM, June 1970
+ The above paper caused a major revolution in the field of database management and earned Dr. Codd the coveted ACM Turing Award

### An Example of Relation

![An Example of Relation](https://www.cs.montana.edu/~halla/csci440/n3/figure-3-1.png)
### Informal Definition: Relation
+ Informally, a relation looks like a **table** of values. 
+ A relation typically contains a set of **rows**.
+ The data elements in each **row** represent certain facts that correspond to a real-world entity or relationship.
  - In the formal model, rows are called **tuples**.
  
### Informal Definition: Attribute
+ Each column in a relation has a column header that gives an indication of the meaning of the data items in that column.
  - In the formal model, the column **header** is called an attribute name or just attribute)
+ Each attribute of a relation has a name
+ Name, Ssn, Home_phone, Address, Office_phone, Age, and Gpa are attributes of the relation **STUDENT**.
### Informal Definition: Attribute Values
+ Each attribute has a set of values that can be assigned to the attribute.
  - In the formal model, the sets of allowed values are called Domains.
+ For example:
  - The values assigned to SSN is the set of valid nine-digit numbers
  - The values assigned to Name is the set of character strings representing names of persons
  - The values assigned to Age is the possible ages of employees of a company; each must be a value between 15 and 80.
+ Attribute values are (normally) required to be indivisible:
  - attribute cannot be multi-valued attribute
    + For example: the value of an attribute can be an account number, but cannot be a set of account numbers.
+ attribute cannot be composite attribute
+ In the formal model, the indivisible property is called to be atomic.

### Informal Definition: Attribute Data Type
+ A **data type** or **format** is also specified for allowed values of an attribute(i.e. domain).
+ For example:
  - The data type of Name is string 
  - The data type of Ssn is string 
  - The data type of Age is integer 
  - The data type of Gpa is real
+ Different attributes can have the same data type.
  - Name and Ssn have the same data type – string
### Informal Definition: NULL Value
+ The special value **NULL** can be a value of any attribute
+ The NULL value can have several meanings
  - value unknown
    + For example: The home phone of student Dick is NULL
  - value value exists but is not available
    + For example: The age of student John is NULL
  - attributes does not apply to this tuple 
    + For example, Visa_Status attribute can only be applied to international students.
+ The null value causes complications in the definition of many operations.
  - For example: how to compare two NULL values?
  - We shall ignore the effect of null values in our main presentation and consider their effect later
### Informal Definition: Key
+ **Key**: a set of attributes such that their values can uniquely identify rows in the table.
+ In the STUDENT table, SSN is the key.
+ Sometimes row-ids or sequential numbers are assigned as keys to identify the rows in a table.
  - Called **artificial key** or surrogate key

### Formal Definition: Relation Schema
+ **Relation Schema** is denoted by R(A1,A2,...,An), where
  - R is the name of the relation
  - A1, A2, ..., An are attributes of the relation
+ Example: **STUDENT(Name, Ssn, Home_phone, Address, Office_phone, Age, Gpa)** where
  - STUDENT is the relation name
  - Defined over 7 attributes: Name, Ssn, ..., Age, Gpa
+ Each attribute has a **domain** or a set of valid values.
  - For example, the domain of Home_phone is 10 digit numbers
### Formal Definition: Domain
+ A domain has a logical definition
  - Example: “USA_phone_numbers” are the set of 10 digit phone numbers valid in the U.S.
+ A domain also has a data-type or a format defined for it.
  - The USA_phone_numbers may have a format: (ddd)ddd-dddd where each d is a decimal digit.
  - Dates have various formats such as year, month, date formatted as yyyy-mm-dd, or as dd mm,yyyy etc.
+ The attribute name designates the role played by a domain in a relation:
  - Used to interpret the meaning of the data elements corresponding to that attribute
  - Example: The domain Date may be used to define two attributes named “Invoice-date” and “Payment-date” with different meanings
  
### Formal Definition: Tuple
+ A tuple is an ordered set of values (enclosed in angled brackets ‘< ... >’)
+ Each value is derived from an appropriate domain.
+ A row in the STUDENT relation is a 7-tuple and would consist of 7 values, for example:
  - <“Benjamin Bayer”, 305-61-2435, 373-1616, “2918 Bluebonnet Lane”, NULL, 19, 3.21>
  - This is called a 7-tuple as it has 7 values
  - A tuple(row) in the STUDENT relation
+ A relation is a set of such tuples(rows)

+ We refer to component values of a tuple t by:
  - t[Ai] or t.Ai
  - This is the value vi of attribute Ai for tuple t
  - Similarly, t[Au, Av, ..., Aw] refers to the subtuple of t containing the values of attributes Au, Av, ..., Aw, respectively in t 
### Formal Definition: State
+ Relation(or Relation state): a subset of the Cartesian product of the domains of its attributes
  - denoted by r(R) = {t1, t2, ..., tm} where each ti is an n-tuple
  - ti ={v1,v2,...,vn}where each vi is an element of domain (Ai) or NULL
  
### Formal Definition: Example
+ Let R(A1, A2) be a relation schema:
  - Let dom(A1) = {0,1}
  - Let dom(A2) = {a,b,c}
+ Then: dom(A1) X dom(A2) is all possible combinations: {<0,a> , <0,b> , <0,c>, <1,a>, <1,b>, <1,c> }
+ The relation state r(R) ⊂ dom(A1) X dom(A2)
  - For example: r(R) could be {<0,a> , <0,b> , <1,c> }
  - this is one possible state (or “population” or “extension”) r of the relation R, defined over A1 and A2.
  - It has three 2-tuples: <0,a> , <0,b> , <1,c>
### Relational Database Schema
+ Relational Database Schema S = {R1, R2, ..., Rn}, where
  - S: the name of the whole database schema
  - R1, R2, ..., Rn are the names of the individual relation schemas within the database S
  - a set of relation schemas that belong to the same database
+ That is, a relational database schema is a set of relation schemas that belong to the same database

### Definition Summary

![Definition Summary](https://www.cs.montana.edu/~halla/csci440/n3/figure-3-5.png)

|Informal Term|Formal Term|
|---|---|
|Table|Relation|
|Column Header|Attribute|
|All possible column values|Domain|
|Row|Tuple|
|Table Definition|Schema of a Relation|
|Populated Table|State of Relation|






## Characteristics
### Relation Characteristics
+ Ordering of tuples in a relation r(R):
  - The tuples are not considered to be ordered, even though they appear to be in the tabular form
+ Ordering of attributes in a relation schema R (and of values within each tuple):
  - We will consider the attributes in R(A1, A2, ..., An) and the values in t =< v 1, v 2, ..., vn > to be ordered .
  - However, a more general alternative definition of relation does not require this ordering
+ Values in a tuple
  - All values are considered atomic (indivisible).
+ Each value in a tuple must be from the domain of the attribute for that column
  - If tuple t = <v1, v2, ..., vn> is a tuple (row) in the relation state r of R(A1, A2, ..., An)
  - Then each vi must be a value from dom(Ai)
+ A special null value is used to represent values that are unknown or inapplicable to certain tuples

### Relation with Different Order
+ Constraints: are conditions that must hold on all valid relation states
+ Three main types of constraints in the relational model:
  - **Key** constraints
  - **Entity** integrity constraints 
  - **Referential** integrity constraints
+ Another implicit constraint is the domain constraint
  - Every value in a tuple must be from the domain of its attribute (or it could be null, if allowed for that attribute)
#### Key Constraints
+ **Superkey** of R: a set of attributes SK of R with the following condition
  - No two tuples in any valid relation state r(R) will have the same value for SK
  - That is, for any distinct tuples t1 and t2 in r(R), t1[SK] ≠ t2[SK]
  - This condition must hold in any valid state r(R)
  
+ **Key** of R: a “minimal” superkey
  - A key is a superkey K such that removal of any attribute from K results in a set of attributes that is not a superkey (does not possess the superkey uniqueness property)
+ Example
  - Consider the CAR relation schema: CAR(State, Reg , SerialNo, Make, Model, Year)
  - CAR has two keys:
    + Key1 = {State, Reg#}
    + Key2 = {SerialNo}
  - Both are also superkeys of CAR
  - {SerialNo, Make} is a superkey but not a key
  - In general:
    + Any key is a superkey (but not vice versa)
    + Any set of attributes that includes a key is a superkey
    + A minimal superkey is also a key
    
####    

## Constraint
### Relational Integrity Constraints

### Key Constraints
### Entity Integrity
### Referential Integrity
### Semantic Integrity Constraints
## Relational Schema Diagram
## Operations on Relation 
### Concepts
### Constraint Violation
  
  
