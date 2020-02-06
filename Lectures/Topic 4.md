# CSCI 360 Spring 2020
# Dr. Ning Zhang
# Topic 4: The Relational Algebra and Calculus (Chapter 8)

# Outline
+ Relational Algebra
  - Unary Relational Operations
  - Relational Algebra Operations From Set Theory Binary Relational Operations
  - Additional Relational Operations
  - Examples of Queries in Relational Algebra
+ Relational Calculus
  - Tuple Relational Calculus 
  - Domain Relational Calculus
+ Example Database Application (COMPANY)
+ Overview of the QBE language (appendix D)




## Relational Algebra

### History
+ Muhammad ibn Musa al-Khwarizmi (800-847 CE) wrote a book titled al-jabr about arithmetic of variables
  - Book was translated into Latin
  - Its title (al-jabr) gave Algebra its name.
+ Al-Khwarizmi called variables “shay”
  - “Shay” is Arabic for “thing”
  - Spanish transliterated “shay” as “xay” (“x” was “sh” in Spain)
+ Where does the word Algorithm come from?
  - Algorithm originates from “al-Khwarizmi”
  - Reference: PBS (http://www.pbs.org/empires/islam/innoalgebra.html)
  
### Overview

+ Relational algebra is the basic set of operations for the relational model
+ These operations enable a user to specify basic retrieval requests (or queries)
+ The result of an operation is a new relation, which may have been formed from one or more input relations
  - This property makes the algebra “closed” (all objects in relational algebra are relations)
  - The new relations can be further manipulated using operations of the same algebra
+ A sequence of relational algebra operations forms a **relational algebra expression**
  - The result of a relational algebra expression is also a relation that represents the result of a database query (or retrieval request)
  
+ Relational Algebra consists of several groups of operations
  - Unary Relational Operations
    + **SELECT** (symbol: σ (sigma))
    + **PROJECT** (symbol: π (pi))
    + **RENAME** (symbol: ρ (rho))
+ Relational Algebra Operations From Set Theory
  - **UNION** ( ∪ ), **INTERSECTION** ( ∩ ), **DIFFERENCE** (or MINUS, −)
  - **CARTESIAN PRODUCT** ( × )
+ Binary Relational Operations
  - **JOIN** (several variations of JOIN exist）
  - **DIVISION**
+ Additional Relational Operations
  - **OUTER JOINS**, **OUTER UNION**
  - **AGGREGATE FUNCTIONS** (These compute summary of information: for example, SUM, COUNT, AVG, MIN, MAX）

#### COMPANY Database Schema
+ All examples discussed below refer to the COMPANY database shown here.
  
 ![company](https://d2vlcm61l7u1fs.cloudfront.net/media%2F500%2F500decc6-d150-4a9e-bca8-b1c219adb1de%2FphpahrxoM.png)
  
 #### COMPANY Database State
 
 ![state](https://d2vlcm61l7u1fs.cloudfront.net/media%2F605%2F6052c100-6ddb-4570-8c74-bff61d413d87%2FphpDnEFxK.png)
 
 ### Unary Relational Operations
 #### SELECT
 + The SELECT operation is used to select a subset of the tuples from a relation based on a selection condition, denoted by σ<sub>\<selection condition\></sub>(R) where
  - the symbol σ (sigma) is used to denote the select operator
  - the selection condition is a Boolean (conditional) expression specified on the attributes of relation R
  - tuples that make the condition true are selected, i.e., appear in the result of the operation
  - tuples that make the condition false are filtered out, i.e., discarded from the result of the operation
+ SELECT operator is applied to **a single relation**
+ SELECT operator is applied to **each tuple individually**
+ For SELECT operation σ<sub>\<selection condition\></sub>(R), < selection condition > is one of the following
  - < attributename >< comparison operator >< constantvalue >
  - < attributename >< comparison operator >< attributename >
  - arbitrarily connected by the above two forms using Boolean operators such as AND, OR, and NOT.
+ If the domain of an attribute is **ordered values**, the comparison operators that can be applied to the attribute are:=,<,≤,>,≥,and ≠.
+ If the domain of an attribute is **unordered values**, the comparison operators that can be applied to the attribute are: =, and ≠. 
+ Some domains may allow additional types of comparison operators, such as SUBSTRING_OF for character strings.

+ examples:
  - Select the EMPLOYEE tuples whose department number is 4: σ<sub>Dno=4</sub>(EMPLOYEE)
  - Select the employee tuples whose salary is greater than $30,000: σ<sub>SALARY>30,000</sub>(EMPLOYEE)
  - Select the employee tuples who either work in department 4 and make over $25,000 per year, or work in department 5 and make over $30,000: σ<sub>(Dno=4 AND Salary>25000) OR (Dno=5 AND Salary>30000)</sub>(EMPLOYEE)
  
+ **SELECT Operation Properties**
  - The SELECT operation σ<sub><selection condition></sub>(R) produces a relation S that has the same schema (same attributes) as R
  - SELECT σ is commutative
    + σ<sub>\<condition1\></sub>(σ<sub>\<condition2\></sub>(R)) = σ<sub>\<condition2\></sub>(σ<sub>\<condition1\><.sub>(R))
  - Because of commutativity property, a cascade (sequence) of SELECT operations may be applied in any order: 
    + σ<sub>\<condition1\></sub>(σ<sub>\<condition2\></sub>(σ<sub>\<condition3\></sub>(R))) = σ<sub>\<condition2\></sub>(σ<sub>\<condition3\></sub>(σ<sub>\<condition1\></sub>(R)))
  - A cascade of SELECT operations may be replaced by a single selection with a conjunction of all the conditions:
    + σ<sub>\<condition1\></sub>(σ<sub>\<condition2\></sub>(σ<sub>\<condition3\></sub>(R))) =σ <sub>\<condition1\> AND \<condition2\> AND \<condition3\></sub>(R)
  - The number of tuples in the result of a SELECT is less than (or equal to) the number of tuples in the input relation R

  
#### PROJECT
+ PROJECT Operation keeps certain columns (attributes) from a relation and discards the other columns
  - PROJECT creates a vertical partitioning
    + The list of specified columns (attributes) is kept in each tuple
    + The other attributes in each tuple are discarded
+ The general form of the project operation is: π<sbu>\<attribute list\></sub>(R) where
  - π (pi) is the symbol used to represent the project operation
  - <attribute list> is the desired list of attributes from relation R, seperated by comma(,)
  
