# CSCI 360 Spring 2020
# Dr. Ning Zhang
# Topic 4: The Relational Algebra and Calculus (Chapter 8)

# Outline
+ Relational Algebra
  - Unary Relational Operations
  - Relational Algebra Operations From Set Theory
  - Binary Relational Operations
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
    + σ<sub>\<condition1\></sub>(σ<sub>\<condition2\></sub>(R)) = σ<sub>\<condition2\></sub>(σ<sub>\<condition1\></sub>(R))
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
  - \<attribute list\> is the desired list of attributes from relation R, seperated by comma(,)
+ Example: To list each employee’s first and last name and salary, the following is used:
  - π<sub>Lname, Fname Salary</sub>(EMPLOYEE)

+ **PROJECT Operation Properties**
  - The project operation removes any duplicate tuples
    + This is because the result of the project operation must be a set of tuples. Mathematical sets do not allow duplicate elements.
  - The number of tuples in the result of projection π<sub>list</sub>(R) always less or equal to the number of tuples in R
    + If the list of attributes includes a key of R, then the number of tuples in the result of PROJECT is equal to the number of tuples in R
  - PROJECT is NOT commutative
    + π<sub>\<list1\></sub>(π<sub>\<list2\></sub>(R)) = π<sub>\<list1\></sub>(R) as long as \<list2\> contains the attributes
 in \<list1\>
  
![select project](http://www.brainkart.com/media/extra/OG7pA6u.jpg)
  
  
#### Relational Algebra Expressions
+ Two ways to apply several relational algebra operations one after the other
  - Relational algebra expression: nesting the operations
  - Apply one operation at a time and create intermediate result relation
    + Names must be given to the relations holding the intermediate results.
+ Example: To retrieve the first name, last name, and salary of all employees who work in department number 5, we must apply a select and a project operation
  - Relational algebra expression: 
    + π<sub>Fname,Lname,Salary</sub> (σ<sub>DNO=5</sub>(EMPLOYEE))
  - Sequence of operations
    + DEP5_EMPS ← σ<sub>DNO=5</sub>(EMPLOYEE)
    + RESULT ← π<sub>Fname,Lname,Salary</sub> (DEP5_EMPS)
+ The expression DEP5_EMPS ← σ<sub>DNO=5</sub>(EMPLOYEE) means:
  - The selection result of the operation σ<sub>DNO=5</sub>(EMPLOYEE) will be saved in a relation.
  - The relation is called DEP5_EMPS.
  - The attributes of the DEP5_EMPS are the same as the attributes in the relation EMPLOYEE
+ The expression RESULT ← π<sub>Fname,Lname,Salary</sub> (DEP5_EMPS) means
  - The projection result of the operation π<sub>Fname,Lname,Salary</sub> (DEP5_EMPS) will be stored in a relation.
  - The relation is called RESULT
  - The attributes of RESULT are Fname, Lname, and Salary.
#### RENAME Operation
+ In some cases, we may want to rename the attributes of a relation or the relation name or both
  - Useful when a query requires multiple operations
  - Necessary in some cases (see JOIN operation later)
+ RENAME operation: denoted by ρ 
  - Give a new schema to a relation R by changing(rho)
    + attribute names {A1, A2, A3, ..., An} to {B1, B2, B3, ..., Bn}
      - ρ<sub>(B1,B2,B3,...,Bn)</sub>(R)
    + relation name to S
      - ρ<sub>S</sub>(R)
    + both the attribute names and relation names
      - ρ<sub>S(B1,B2,B3,...,Bn)</sub>(R)
+ RENAME Operation: Shorthand Notation
  - For convenience, we also use a shorthand for renaming attributes in an intermediate relation
    + RESULT ← π<sub>Fname,Lname,Salary</sub>(DEP5_EMPS)
      - RESULT will have the same attribute names as DEP5_EMPS (same attributes as EMPLOYEE)
    + RESULT(SSN) ← π<sub>SUPERSSN</sub>(DEP5_EMPS)
      - RESULT will have only one attribute SSN (same attributes as SUPERSSN in EMPLOYEE)

### Operations for Set Theory
#### Basic Relational Algebra Operations from Set Theory
+ UNION Operation, denoted by ∪
  - The result of R∪S, is a relation that includes all tuples that are either in R or in S or in both R and S
  - Duplicate tuples are eliminated
+ INTERSECTION operation: denoted by ∩
  - The result of the operation R∩S, is a relation that includes all tuples that are in both R and S
+ SET DIFFERENCE operation: denoted by −
  - The result of R − S, is a relation that includes all tuples that are in R but not in S
#### Type Compatibility
+ R1(A1, A2, ..., An) and R2(B1, B2, ..., Bn) are type compatible if:
  - they have the same number of attributes, and
  - the domains of corresponding attributes are type compatible (i.e. dom(Ai)=dom(Bi) for i=1, 2, ..., n)
  - Two relations may have different names and different attribute names
+ Type Compatibility of operands is required for the binary set operations: UNION, INTERSECTION, and SET DIFFERENCE.
+ The resulting relation for R1∪R2 (also for R1∩R2, or R1 − R2) has the same attribute names as the first operand relation R1 (by convention)

#### Union Operation Example
+ To retrieve the social security numbers of all employees who either work in department 5 (RESULT1 below) or directly supervise an employee who works in department 5 (RESULT2 below)
  - DEP5_EMPS ← σ<sub>DNO=5</sub>(EMPLOYEE)
  - RESULT1 ← π<sub>SSN</sub>(DEP5_EMPS)
  - RESULT2(SSN) ← π<sub>SUPERSSN</sub>(DEP5_EMPS)
  - RESULT ← RESULT1 ∪ RESULT2
+ The union operation produces the tuples that are in either RESULT1 or RESULT2 or both

![Union Operation Example](https://image1.slideserve.com/3415568/figure-6-3-results-of-the-union-operation-result-result1-result2-l.jpg)


#### UNION, INTERSECTION, and MINUS Example

![UNION, INTERSECTION, and MINUS Example](https://slideplayer.com/slide/3280030/11/images/2/Set+Based%3A+UNION%2C+INTERSECTION%2C+DIFFERENCE.jpg)


#### Properties of UNION, INTERSECT, and DIFFERENCE
+ **Commutative**: Both UNION and INTERSECTION are commutative
  - R ∪ S = S ∪ R
  - R ∩ S = S ∩ R
+ Associative: Both UNION and INTERSECTION are associative
  - R ∪ (S ∪ T) = (S ∪ R) ∪ T
  - R ∩ (S ∩ T) = (S ∩ R) ∩ T
+ The MINUS operation is neither commutative nor associative, i.e., in general
  - R − S ≠ S − R
  - R − (S − T) ≠ (S − R) − T
  
#### Binary Relational Operations: CARTESIAN PRODUCT

+ CARTESIAN (or CROSS) PRODUCT Operation
  - This operation is used to combine tuples from two relations in a combinatorial fashion
  - Denoted by R(A1, A2, ..., An) x S(B1, B2, ..., Bm)
  - Result is a relation Q with degree n + m attributes: Q(A1, A2, ..., An, B1, B2, ..., Bm), in that order.
  - The resulting relation state has one tuple for each combination of tuples-one from R and one from S.
  - Hence, if R has n<sub>R</sub> tuples (denoted as |R| = n<sub>R</sub> ), and S has n<sub>S</sub> tuples, then R × S will have n<sub>R</sub> ∗ n<sub>S</sub> tuples
  - The two operands do NOT have to be “type compatible”
+ Generally, CARTESIAN PRODUCT is **not a meaningful operation**
  - For example:
    + FEMALE_EMPS ← σ<sub>SEX =′F′</sub>(EMPLOYEE)
    + EMPNAMES ← π<sub>FNAME,LNAME,SSN</sub>(FEMALE_EMPS)
    + EMP_DEPENDENTS ← EMPNAMES×DEPENDENT
  - EMP_DEPENDENTS will contain every combination of EMPNAMES and DEPENDENT **NO MATTER they are actually related or not**.
+ CARTESIAN PRODUCT become meaningful when followed by other operations
  - To keep only combinations where the DEPENDENT is related to the EMPLOYEE, we add a SELECT operation
    + FEMALE_EMPS ← σ<sub>SEX =′F′</sub>(EMPLOYEE)
    + EMPNAMES ← π<sub>FNAME,LNAME,SSN</sub>(FEMALE_EMPS)
    + EMP_DEPENDENTS ← EMPNAMES×DEPENDENT
    + ACTUAL_DEPS ← σ<sub>SSN=ESSN</sub> (EMP_DEPENDENTS)
    + RESULT ← π<sub>FNAME ,LNAME ,DEPENDENTNAME</sub> (ACTUAL_DEPS)
    
  - RESULT will now contain the name of female employees and their dependents
  
  ![](https://slideplayer.com/slide/15218896/92/images/34/Figure+8.5+The+CARTESIAN+PRODUCT+%28CROSS+PRODUCT%29+operation..jpg)
  ![](https://slideplayer.com/slide/15218896/92/images/35/Figure+8.5+%28continued%29+The+CARTESIAN+PRODUCT+%28CROSS+PRODUCT%29+operation..jpg)
  ![](https://slideplayer.com/slide/12404110/74/images/36/Figure+8.5+%28continued%29+The+CARTESIAN+PRODUCT+%28CROSS+PRODUCT%29+operation..jpg)
  
  
  
#### Binary Relational Operations: JOIN
+ JOIN Operation (denoted by ⋈) 
  - The sequence of CARTESIAN PRODECT followed by SELECT is used quite commonly to identify and select related tuples from two relations.
  - JOIN combines this sequence into a single operation
  - This operation is very important for any relational database with more than a single relation, because it allows us combine related tuples from various relations
+ Syntax
  - The general form of a join operation on two relations R(A<sub>1</sub> , A<sub>2</sub> , ..., A<sub>n</sub>) and S(B<sub>1</sub> , B<sub>2</sub> , ..., B<sub>m</sub>) is **R ⋈<sub>\<join condition\></sub> S**, where R and S can be any relations that result from general relational algebra expressions
+ Example: Suppose that we want to retrieve the name of the manager of each department. 
  - To get the manager’s name, we need to combine each DEPARTMENT tuple with the EMPLOYEE tuple whose SSN value matches the MGRSSN value in the department tuple.
  - We do this by using the join operation: DEPT_MGR ← DEPARTMENT⋈<sub>MGRSSN =SSN</sub>EMPLOYEE
  - MGRSSN=SSN is the join condition
    + Combines each department record with the employee who manages the department
    + The join condition can also be specified as: DEPARTMENT.MGRSSN= EMPLOYEE.SSN
  
  ![joint](https://lh5.ggpht.com/-c03wuyD6Ods/VRGf3gURduI/AAAAAAABf6c/ykqMUYs6-Nw/clip_image038_thumb.gif?imgmax=800)
  
+ JOIN Properties
  - Consider the following JOIN operation: R(A<sub>1</sub> , A<sub>2</sub> , ..., A<sub>n</sub>) ⋈<sub>R.A<sub>i</sub>=S.B<sub>j</sub></sub> S(B<sub>1</sub> , B<sub>2</sub> , ..., B<sub>m</sub>)
  - Result is a relation Q with degree n + m attributes Q(A<sub>1</sub>, A<sub>2</sub>, ..., A<sub>n</sub>, B<sub>1</sub>, B<sub>2</sub>, ..., B<sub>m</sub>), in that order.
  - The resulting relation state has one tuple for each combination of tuples – r from R and s from S, but only if they satisfy the join condition r[A<sbu>i</sub>]=s[B<sub>j</sub>]
  
  - Hence, if R has n<sub>R</sub> tuples, and S has n<sub>S</sub> tuples, then the join result will generally have less than n<sub>R</sub>∗n<sub>S</sub> tuples
  - Only related tuples (based on the join condition) will appear in the result
+ **Theta JOIN**
  - Theta JOIN: the general case of JOIN operation, denoted by R(A<sub>1</sub> , A<sub>2</sub> , ..., A<sub>n</sub>) ⋈<sub>theta</sub> S(B<sub>1</sub> , B<sub>2</sub> , ..., B<sub>m</sub>)
    + The join condition is called theta (θ)
    + Theta can be any general boolean expression on the attributes of R and S; for example: R.A<sub>i</sub> < S.B<sub>j</sub> AND (R.A<sub>k</sub> = S.B<sub>l</sub> OR R.A<sub>p</sub> < S.B<sub>q</sub>)
    + Most join conditions involve one or more equality conditions “AND”ed together; for example: R.A<sub>i</sub> < S.B<sub>j</sub> AND R.A<sub>k</sub> = S.B<sub>l</sub> AND R.A<sub>p</sub> < S.B<sub>q</sub>
  - Theta JOIN example: CAR⋈<sub>CarPrice≥BoatPrice</sub>BOAT
  
+ **EQUIJOIN**
  - The most common use of join involves join conditions with equality comparisons only
  - The only comparison operator used in EQUIJOIN is =
  - In the result of an EQUIJOIN we always have one or more pairs of attributes (whose names need NOT be identical) that have identical values in every tuple.
  - The JOIN seen in the previous example was an EQUIJOIN.
+ **NATURAL JOIN**
  - NATURAL JOIN Operation (denoted by ∗)
    + It requires that the two join attributes, or each pair of corresponding  join attributes, have the same name in both relations.
    + If this is not the case, a renaming operation is applied first.
    + It was created to get rid of the second (superfluous) attribute in an EQUIJOIN condition because one of each pair of attributes with identical values is superfluous.
  - Example: Q ← R(A,B,C,D) * S(C,D,E)
    + The implicit join condition includes each pair of attributes with the same name, “AND”ed together R.C=S.C AND R.D=S.D
    + Result keeps only one attribute of each such pair Q(A, B, C, D, E)
  - Example: To apply a natural join on the DNUMBER attributes of DEPARTMENT and DEPT_LOCATIONS, it is sufficient to write **DEPT_LOCS ← DEPARTMENT * DEPT_LOCATIONS** 
    + Only attribute with the same name is DNUMBER
    + An implicit join condition is created based on this attribute: DEPARTMENT.DNUMBER=DEPT_LOCATIONS.DNUMBER
    
    ![dept_locs](https://lh5.ggpht.com/-OOULiwSNjBk/VRGgIO_ThnI/AAAAAAABf7U/DoTGrBju9xI/clip_image045_thumb.gif?imgmax=800)
    
#### Complete Set of Relational Operations
+ The set of operations including SELECT, PROJECT, UNION, DIFFERENCE, RENAME, and CARTESIAN PRODUCT is called a complete set because
  - any other relational algebra expression can be expressed by a combination of these five operations
+ For example:
  - R∩S = (R∪S) − ((R − S)∪(S − R))
  - R ⋈ <sub>\<join condition\></sub> S = σ<sub>\<join condition\></sub>(R×S)
  
#### Binary Relational Operations: DIVISION
+ DIVISION Operation, denoted by (÷)
  - The division operation is applied to two relations R and S, denoted by R(Z) ÷ S(X) where
    + Z is the set of attributes of R
    + X is the set of attributes of S
    + X is a subset of Z
    + Let Y be the set of attributes of R that are not attributes of S, i.e, Y = Z - X
  - The result of DIVISION is a relation T(Y) that includes a tuple t if tuples t<sub>R</sub> appear in R with t<sub>R</sub>[Y] = t, and with t<sub>R</sub>[X] = t<sub>S</sub> for every tuple t<sub>S</sub> in S.
  - For a tuple t to appear in the result T of the DIVISION, the values in t must appear in R in combination with every tuple in S.
  - DIVISION Example
  
  ![division](https://www.cs.montana.edu/~halla/csci440/n6/figure-6-8.png)
  
  
  ![operations of relational algebra](https://www.cs.montana.edu/~halla/csci440/n6/table-6-1.png)
  
#### Query Tree Notation
+ Query Tree
  - An internal data structure to represent a query
  - Standard technique for estimating the work involved in executing the query, the generation of intermediate results, and the optimization of execution
  - Nodes stand for operations like selection, projection, join, renaming, division, ...
  - Leaf nodes represent base relations
  - A tree gives a good visual feel of the complexity of the query and the operations involved
  - Algebraic Query Optimization consists of rewriting the query or modifying the query tree into an equivalent tree
  
  
  ![query tree](http://www.brainkart.com/media/extra/xzHeoCO.jpg)
  
### Additional Relational Operations
#### Aggregate Functions and Grouping
+ A type of request that cannot be expressed in the basic relational algebra is to specify mathematical **aggregate functions** on collections of values from the database
  - Examples of such functions include retrieving the average or total salary of all employees or the total number of employee tuples
  - These functions are used in simple statistical queries that summarize information from the database tuples
  - Common functions applied to collections of numeric values include **SUM, AVERAGE, MAXIMUM, and MINIMUM**
  - The **COUNT** function is used for counting tuples or values
+ Aggregate Function Operation
  - Syntax: <sub>\<groupting attributes\></sub> ℑ <sub>\<function list\></sub>(R)
  - Use of the Aggregate Functional operation ℑ
    + retrieves the maximum salary value from the EMPLOYEE relation ℑ <sub>MAX Salary</sub>(EMPLOYEE)
    + retrieves the minimum Salary value from the EMPLOYEE relation ℑ <sub>MIN Salary</sub>(EMPLOYEE)
    + retrieves the sum of the Salary from the EMPLOYEE relation ℑ <sub>SUM Salary</sub>(EMPLOYEE)
    + computes the count (number) of employees and their average salary ℑ<sub>COUNT SSN, AVERAGE Salary</sub>(EMPLOYEE)
      - Note: count just counts the number of rows, without removing duplicates
+ Using Group with Aggregation
  - The previous examples all summarized one or more attributes for a set of tuples
    + Maximum Salary or Count (number of) Ssn
  - Grouping can be combined with Aggregate Functions
    + Example: For each department, retrieve the DNO, COUNT SSN, and AVERAGE SALARY
    + A variation of aggregate operation ℑ allows this
      - Grouping attribute placed to left of symbol
      - Aggregate functions to right of symbol
      - For example: DNOℑCOUNT<sub>SSN, AVERAGE Salary</sub>(EMPLOYEE)
      - Above operation groups employees by DNO (department number) and computes the count of employees and average salary per departmen
    
    ![aggregate function operation](https://www.cs.montana.edu/~halla/csci440/n6/figure-6-10.png)
    
    ![aggregate function operation](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcQi6Avc5-YsN-tVu8VyGaAyeBqmrArkcXRKUFve8R0KutcAWx3D)
    
    
+ Recursive Closure Operation
  - Another type of operation that, in general, cannot be specified in the basic original relational algebra is recursive closure
    + This operation is applied to a recursive relationship, i.e., an entity type has a relationship type with itself.
  - An example of a recursive operation is to retrieve all SUPERVISEES of an EMPLOYEE **e** at all levels - that is,
    + all EMPLOYEE e" directly supervised by e';
    + all employees e”" directly supervised by each employee e”;
    + and so on
  - Although it is possible to retrieve employees at each level and then take their union, we cannot, in general, specify a query such as “retrieve the supervisees of ‘James Borg’ at all levels” without utilizing a looping mechanism
    + The SQL3 standard includes syntax for recursive closure
  - Example
  
  ![Recursive Closure Operation](https://image1.slideserve.com/2632734/recursive-closure-operation1-l.jpg)
  
  
+ OUTER JOIN Operation
  - In NATURAL JOIN and EQUIJOIN
    + Tuples without a matching (or related) tuple are eliminated from the join result
    + Tuples with null in the join attributes are also eliminated
    + This amounts to loss of information
  - A set of operations, called OUTER joins, can be used when we want to keep all the tuples in R, or all those in S, or all those in both relations in the result of the join, regardless of whether or not they have matching tuples in the other relation.
  - **LEFT OUTER JOIN Operation**
    + keeps every tuple in the first or left relation R in R ⋈ S; if no matching tuple is found in S, then the attributes of S in the join result are filled or “padded” with null values.
    + denoted by  ⟕
  - **RIGHT OUTER JOIN Operation**
    + keeps every tuple in the second or right relation R in R ⋈ S; if no matching tuple is found in R, then the attributes of R in the join result are filled or “padded” with null values.
    + denoted by  ⟖ 
  - **FULL OUTER JOIN Operation**
    + keeps all tuples in both the left and the right relations when no matching tuples are found, padding them with null values as needed
    + denoted by ⟗ 
    
  - Example: 
    + TEMP ← (EMPLOYEE ⟕ <sub>Ssn=Mgr\_ssn</sub>DEPARTMENT)
    + RESULT ← π<sub>Fname, Minit, Lname</sub>(TMEP)
    
    ![left join](https://slideplayer.com/slide/15218896/92/images/63/Figure+8.12+The+result+of+a+LEFT+OUTER+JOIN+operation..jpg)

+ OUTER UNION Operation  
  - The outer union operation was developed to take the union of tuples from two relations if the relations are not type compatible.
  - This operation will take the union of tuples in two relations R(X, Y) and S(X, Z) that are partially compatible, meaning that only some of their attributes, say X, are type compatible.
  - The attributes that are type compatible are represented only once in the result, and those attributes that are not type compatible from either relation are also kept in the result relation T(X, Y, Z).
  - Example
    + An outer union can be applied to two relations whose schemas are **STUDENT(Name, SSN, Department, Advisor)** and **INSTRUCTOR(Name, SSN, Department, Rank)**
    + Tuples from the two relations are matched based on having the same combination of values of the shared attributes - Name, SSN, Department.
    + If a student is also an instructor, both Advisor and Rank will have a value; otherwise, one of these two attributes will be null.
    + The result relation STUDENT_OR_INSTRUCTOR will have the following attributes: **STUDENT_OR_INSTRUCTOR (Name, SSN, Department, Advisor, Rank)**
  
#### Query Examples 1
+ Retrieve the name and address of all employees who work for the ‘Research’ department
+ Procedural form:
  - RESEARCH_DEPT ← σ<sub>Dname='Research'</sub>DEPARTMENT
  - RESEARCH_EMPS ← RESEARCH_DEPT ⋈ <sub>Dnumber=Dno</sub>EMPLOYEE
  - RESULT ← π<sub>Fname,Lname,Address</sub>(RESEARCH_EMPS)
+ Single Expression
  - π<sub>Fname,Lname,Address</sub>(σ<sub>Dname='Research'</sub>(DEPARTMENT⋈ <sub>Dnumber=Dno</sub>EMPLOYEE))
    
#### Query Examples 2
+ Retrieve the names of employees who have no dependents
+ Procedural form:
  - ALL_EMPS ← π<sub>SSN</sub>(EMPLOYEE)
  - EMPS_WITH_DEPS(Ssn) ← π<sub>Essn</sub>(DEPENDENT)
  - EMPS_WITHOUT_DEPS ← ALL_EMPS − EMPS_WITH_DEPS
  - RESULT ← π<sub>Lname,Fname</sub>(EMPS_WITHOUT_DEPS * EMPLOYEE)
+ Single expression:
  - π<sub>Lname,Fname</sub>((π<sub>Ssn</sub>(EMPLOYEE) − π<sub>Essn</sub>(DEPENDENT))*EMPLOYEE)
