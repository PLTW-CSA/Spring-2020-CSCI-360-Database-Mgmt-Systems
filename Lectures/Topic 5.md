# CSCI 360 Spring 2020
# Dr. Ning Zhang
# Topic 5: SQL-99: Schema Definition, Constraints, and Queries and Views

# Outline
+ Structured Query Language (SQL)
  - SQL Data Types
  - Data Definition Language
  - Data Manipulation Language
  - Data Query Language
  - SQL functions
  - Category and Schema
  
# 1.SQL Data Types
+ Predefined Data Types
  - Built-in data types
+ Constructed Types
  - Specified using one of SQL’s data type constructors, such as ARRAY, MULTISET, REF, and ROW
+ User-defined Types
  - A type defined by users using SQL statements
## 1.1 Predefined Data Types: Character String Types
### CHARACTER(n) or CHAR(n)
+ used for fixed-length strings
+ n specifies the length
+ if n is omitted, the length is 1
### CHARACTER VARYING(n) or VARCHAR(n)
+ used for varying-length strings
+ n specifies the length
+ n cannot be omitted
### CHARACTER LARGE OBJECT or CLOB
+ to store large nondatabase-structured text objects of varying size and complexity, such as
  - employees’ resumes
  - collection of papers
  - typically, CLOB can store up to 2 or 4 gigabytes of data
## 1.2 Predefined Data Types: Binary String Types
+ A binary string is a sequence of bytes in the same way that a character string is.
+ A binary string is used to hold nontraditional data such as images, audio, and video
+ Literal bit strings are placed between single quotes but preceded by a B to distinguish them from character strings.
  - B’101100’
### BIT(n)
+ used for fixed-length binary strings
+ n specifies the length
+ if n is omitted, the length is 1
### BIT VARYING(n) or VARBIT(n)
+ used for varying-length binary strings
+ n specifies the length
+ n cannot be omitted
### BINARY LARGE OBJECT or BLOB
+ to store large binary values
+ typically, BLOB can store up to 2 or 4 gigabytes of data
## 1.3 Predefined Data Types: Exact Numbers
+ Exact numbers can either be whole integers or have decimal points.
+ Exact numbers can be positive and negative.
+ Exact numbers have precision and scale.
  - Precision determines the maximum total number of decimal digits that can be stored (both to the left and to the right of the decimal point)
  - Scale specifies the maximum number of decimals allowed
+ Literals for exact numbers
  - 123
  - +33.45
  - -334.488
### INTEGER or INT
+ represents countable numbers
+ precision is implementation-specific
### SMALLINT
+ virtually same as INT, but with a smaller precision
### NUMERIC(precision, scale) or DECIMAL(precision, scale)
+ The default for precision is implementation-defined。
+ The default for scale is 0.
+ An error is reported if inserting a value with more figures before the decimal point than column allows.
+ Values with more decimal points than specified will simply be rounded.

## 1.4 Predefined Data Types: Approximate Numbers
+ Approximate numbers are numbers that cannot be represented with absolute precision (or don’t have a precise value)
+ Literals for approximate numbers
  - literals for exact numbers
  - π
  - +1.23E2 (scientific notation)
  - -3.345e001 (scientific notation)
### FLOAT(precision)
+ store floating-point numbers with precision optionally specified by user
### REAL
+ similar to FLOAT, but its precision is fixed
### DOUBLE PRECISION
+ virtually the same as REAL, but with a greater precision
## 1.5 Predefined Data Types: Date and Time Data Types
### DATE
+ Made up of year-month-day in the format yyyy-mm-dd
+ Literal values are represented by single-quoted strings preceded by the keyword DATE. Example: **DATE’2002-09-27’**
### TIME
+ Made up of hour:minute:second in the format hh:mm:ss
+ Literal values are represented by single-quoted strings preceded by the keyword TIME. Example: **TIME’09:12:47’**
### TIME(i)
+ Made up of hour:minute:second plus i additional digits specifying fractions of a second
+ format is hh:mm:ss:ii...i
### TIMESTAMP
+ Has both DATE and TIME components, plus a minimum of six positions for decimal fractions of seconds
+ TIMESTAMP’2002-09-27 09:12:47 648302’
### INTERVAL
+ Specifies a relative value rather than an absolute value
+ Can be DAY/TIME intervals or YEAR/MONTH intervals
+ Can be positive or negative when added to or subtracted from an absolute value, the result is an absolute value
### TIME/TIMESTAMP WITH TIMEZONE
+ similar to TIME/TIMESTAMP
+ includes an additional six positions for specifying the displacement from the standard universal time zone
## 1.6 Predefined Data Types: Boolean
+ In SQL, because of the presence of NULL values, a three-valued logic is used, i.e. TRUE, FALSE, or UNKNOWN.
+ But ORACLE, DB2 and MS SQL don’t have a BOOLEAN data type. Therefore, we cannot create a column of BOOLEAN type.
+ However, BOOLEAN data type can be easily simulated, for example by using a user-defined data type of type VARCHAR that only allows FALSE and TRUE for its values
## 1.7 Predefined Data Types: User-Defined Types
### CREATE TYPE
+ This statement is used to create a new data type.

  - [Example MS SQL](https://docs.microsoft.com/en-us/sql/t-sql/statements/create-type-transact-sql?view=sql-server-ver15)
    + The following example creates an alias type based on the system-supplied varchar data type.
  ~~~~
  CREATE TYPE SSN  
  FROM varchar(11) NOT NULL ; 
  ~~~~
  - [Example Oracle](https://docs.oracle.com/cd/E11882_01/appdev.112/e25519/create_type.htm#LNPLS01375)
### CREATE DOMAIN
+ This statement is used to create a new domain.
+ A domain is essentially a data type with optional constraints (restrictions on the allowed set of values)
+ Syntax:
  ~~~~
  CREATE DOMAIN name [AS] data_type
  [ DEFAULT expression ]
  [ domain constraints ... ]
  ~~~~
+ Examples
  ~~~~
  CREATE DOMAIN SSN_TYPE AS CHAR(9);
  
  
  CREATE DOMAIN D_NUM AS INT
  CHECK (D_NUM > 0 AND D_NUM < 21);
  ~~~~



# 2.Data Definition Language
+ CREATE statement
  - To make a new database, table, view, index, or users

+ DROP statement
  - To destroy an existing database, table, index, view, or users
+ ALTER statement
  - To modify an existing database object such as table, or views.
  
  
## 2.1 CREATE TABLE
### Create Statement: Syntax

~~~~
CREATE TABLE table_name
(
  {column_name data_type[DEFAULT default_expr ]
                        [column_constraint [ ... ]]
  |table_constraint
  }[,...]
)
~~~~

where column_constraint is

~~~~
[ CONSTRAINT constraint_name ]
{
  NOT NULL | NULL | UNIQUE | PRIMARY KEY |
  CHECK (expression) |
  REFERENCES reftable [ ( refcolumn ) ]
             [ ON DELETE action ] [ ON UPDATE action ]
}
~~~~

and table_constraint is:

~~~~
[ CONSTRAINT constraint_name ]
{
  UNIQUE ( column_name [, ... ] ) |
  PRIMARY KEY ( column_name [, ... ] ) |
  CHECK ( expression ) |
  FOREIGN KEY ( column_name [, ... ] )
              REFERENCES reftable [ ( refcolumn [, ... ] ) ]
              [ [ ON DELETE action ] [ ON UPDATE action ]]
}
~~~~
### Create Statement: Term Explanation

|Term|Explanation|
|----|----|
|table_name|the name (optionally schema0qualified) of the table to be created|
|column_name|the name of a column to be created in the new table|
|data_type|the data type of the column|
|DEFAULT|The DEFAULT clause assigns a default data value for the column whose column definition it appears within. The value is any variable-free expression. The data type of the default expression must match the data type of the column.|
|CONSTRAINT|constraint_name an optional name for a column or table constraint. If not specified, the system generates a name.|
|NOT NULL|The column is not allowed to contain null values.|
|NULL|Thecolumnisallowedtocontainnullvalues.Thisisthedefault.|
|UNIQUE|The UNIQUE constraint specifies that a group of one or more distinct columns of a table may contain only unique values. The behavior of the unique table constraint is the same as that for column constraints, with the additional capability to span multiple columns. Each unique table constraint must name a set of columns that is different from the set of columns named by any other unique or primary key constraint defined for the table. (Otherwise it would just be the same constraint listed twice.) NULLs are permitted.|
|PRIMARY KEY|The primary key constraint specifies that a column or columns of a table may contain only unique (non-duplicate), nonnull values. Only one primary key can be specified for a table, whether as a column constraint or a table constraint. The primary key constraint should name a set of columns that is different from other sets of columns named by any unique constraint defined for the same table.|
|CHECK|The CHECK clause specifies an expression producing a Boolean result which new or updated rows must satisfy for an insert or update operation to succeed. A check constraint specified as a column constraint should reference that column’s value only, while an expression appearing in a table constraint may reference multiple columns.|
|FOREIGN KEY/REFERENCES|Theses clauses specify a foreign key constraint, which specifies that a group of one or more columns of the new table must only contain values which match against values in the referenced column(s) refcolumn of the referenced table reftable. If refcolumn is omitted, the primary key of the reftable is used. The referenced columns must be the columns of a unique or primary key constraint in the referenced table.|
|ON DELETE/ON UPDATE|when the data in the referenced columns is changed, certain actions are performed on the data in this table’s columns. The ON DELETE clause specifies the action to perform when a referenced row in the referenced table is being deleted. Likewise, the ON UPDATE clause specifies the action to perform when a referenced column in the referenced table is being updated to a new value. If the row is updated, but the referenced column is not actually changed, no action is done.|
|action|There are the following possible actions for each ON DELETE or ON UPDATE clause:<ul><li>NO ACTION: Produce an error indicating that the deletion or update would create a foreign key constraint violation. This is the default action.</li><li>RESTRICT: Same as NO ACTION except that this action will not be deferred even if the rest of the constraint is deferrable and deferred.</li><li>CASCADE: Delete any rows referencing the deleted row, or update the value of the referencing column to the new value of the referenced column, respectively.</li><li>SET NULL: Set the referencing column values to null.</li><li>SET DEFAULT: Set the referencing column values to their default value.</li></ul>|
### Create Statement: Database Example

[sql code to create COMPANY relational database](https://github.com/ZhangNingSAU/Spring-2020-CSCI-360-Database-Mgmt-Systems/blob/master/Resources/CompanySchema.sql)

## 2.2 CREATE VIEW
### CREATE VIEW: Concepts
+ Skip this subsection if you are not familiar with SELECT statement.
+ View can be treated as a virtual table, which doesn’t take physical disk space.
+ View definition are stored in databases as compiled queries that dynamically populate data to be used for users’ request.
+ Database users can select rows and columns from a view, join it with other views and tables.
+ Database users cannot feel the difference between views and tables when they retrieve data.
### CREATE VIEW: Syntax
~~~~
CREATE VIEW view_name [(column_name [,...])] AS select_statement;
~~~~
+ column_name list is optional – if it’s skipped, the view columns will be named based on the column names in the SELECT statement.
+ column_name is mandatory if one of the following is true:
  - Any two columns would otherwise have the same name 
  - Any column contains a computed value and no alias
+ All constraints related with this table will also be updated automatically.

### CREATE VIEW: Example
+ Create a view that includes employee’s first name, last name, birth date and salary.
~~~
CREATE VIEW EMP
AS
SELECT FNAME, LNAME, BDATE, SALARY
FROM EMPLOYEE
~~~

## 2.3 DROP TABLE
### DROP Statement
+ Syntax: drop a table
~~~~
DROP TABLE table_name [CASCADE CONSTRAINTS];
~~~~
+ Semantics:
  - Drop the table and all of its tuples from the database.
  - All related constraints are also deleted.
  - CASCADE CONSTRAINTS clause lets you drop a table and all referential integrity constraints that reference the table to be deleted.
  
## 2.4 ALTER TABLE
### ALTER TABLE Statement: Rename a Table
+ Rename an existing table
  - Syntax:
  ~~~
  ALTER TABLE table_name
    RENAME TO new_name;
  ~~~
  - The RENAME forms change the name of a table (or an index,sequence, or view) or the name of an individual column in a table. There is no effect on the stored data.
  - All constraints related with this table will also be updated automatically.
### ALTER TABLE Statement: Add a Constraint
+ Add a constraint to an existing table
  - Syntax：
  ~~~
  ALTER TABLE table_name 
    ADD table_constraint;
  ~~~
  - This form adds a new constraint to a table using the same syntax as CREATE TABLE.
### ALTER TABLE Statement: Drop a Constraint
+ Remove a constraint from an existing table
  - Syntax
  ~~~
  ALTER TABLE table_name
    DROP CONSTRAINT constraint_name [RESTRICT | CASCADE];
  ~~~
  - This form drops constraints on a table.
### ALTER TABLE Statement: Add a Column
+ Add a column to an existing table
  - Syntax
  ~~~
  ALTER TABLE table_name
    ADD [COLUMN] column_definition;
  ~~~
  - column_definition is the same as the column definition in the CREATE TABLE statement.
  - The new attribute will have NULLs in all the tuples of the relation right after the command is executed; hence, the NOT NULL constraint is not allowed for such an attribute.
### ALTER TABLE Statement: Drop a Column
+ Remove a column from an existing table
  - Syntax
  ~~~
  ALTER TABLE table_name
    DROP [COLUMN] column_name [RESTRICT | CASCADE];
  ~~~
  - drops a column from a table. Indexes and table constraints involving the column will be automatically dropped as well.
  - You will need to say CASCADE if anything outside the table depends on the column, for example, foreign key references or views.
### ALTER TABLE Statement: Alter a Column
+ Change the definition of a column on an existing table
  - Syntax
  ~~~
  ALTER TABLE table_name
    ALTER [COLUMN] column_name [SET DEFAULT expression | DROP DEFAULT];
  ~~~
  - These forms set or remove the default value for a column.
  - The default values only apply to subsequent INSERT commands; they do not cause rows already in the table to change.
# 3.Data Manipulation Language
## 3.1 INSERT
### INSERT Statement
+ Insert new rows into a table
  - Syntax
  ~~~
  INSERT INTO table_name [ ( column [, ...] ) ]
  DEFAULT VALUES |
  VALUES ( { expression | NULL | DEFAULT } [, ...] ) |
  Query
  ~~~
  - DEFAULT VALUES: All columns will be filled with their default values.
  - Query is a SELECT statement that supplies the rows to be inserted.
  - One can insert a single row at a time or several rows as a result of a query.
+ Inserting rows into a table obeys certain rules and restrictions.
  - All column values have to be of same or at least compatible data types and sizes with corresponding column definitions.
  - The insertion will fail if it violates an integrity constraint, such as inserting NULL values to NOT NULL columns, inserting duplicate values for UNIQUE or PRIMARY KEY columns.
  - If a row violates the integrity constraint, then the whole insert will fail.
+ Examples
  - Example 1:
  ~~~
  INSERT INTO EMPLOYEE
  VALUES (’Richard’,’K’,’Marini’, ’653298653’, DATE’12-30-52’,’98 Oak Forest,Katy,TX’, ’M’, 37000,’987654321’, 4);
  ~~~
    + Attribute values should be listed in the same order as the attributes were specified in the CREATE TABLE command.
  - Example 2:
  ~~~
  INSERT INTO EMPLOYEE (FNAME, LNAME, SSN) VALUES (’Richard’, ’Marini’, ’653298653’);
  ~~~
    + Specifies explicitly the attribute names that correspond to the values in the new tuple.
    + Attributes with NULL values or default values can be left out.
  - Example 3:
    + Suppose we want to create a temporary table that has the name, number of employees, and total salaries for each department.
  ~~~
  CREATE TABLE DEPTS_INFO (
  DEPT_NAME VARCHAR(10),
  NO_OF_EMPS INTEGER,
  TOTAL_SAL INTEGER);
  ~~~
    + The table DEPTS_INFO is loaded with the summary information retrieved from the database by the query below:
  ~~~
  INSERT INTO DEPTS_INFO (DEPT_NAME, NO_OF_EMPS, TOTAL_SAL)
  SELECT DNAME, COUNT (*), SUM (SALARY)
  FROM DEPARTMENT, EMPLOYEE
  WHERE DNUMBER=DNO
  GROUP BY DNAME;
  ~~~
    + Note: The DEPTS_INFO table may not be up-to-date if we change the tuples in either the DEPARTMENT or the EMPLOYEE relations after issuing the query. We have to create a view (see later) to keep such a table up to date.
## 3.2 DELETE
### DELETE Statement
+ Delete rows from a table
  - Syntax
  ~~~
  DELETE FROM table [ WHERE condition ]
  ~~~
  - DELETE deletes rows that satisfy the WHERE clause from the specified table.
  - If the WHERE clause is absent, the effect is to delete all rows in the table. The result is a valid, but empty table.
  - condition: A value expression that returns a value of type boolean that determines the rows which are to be deleted.
  - PRIMARY KEY, UNIQUE, NOT NULL, or CHECK constraints would not prevent you from deleting a row.
  - You would not be able to delete a row that contains a column referenced by another column unless the referential integrity constraint has the ON DELETE CASCADE/SET NULL option.
+ Examples
~~~~
DELETE FROM EMPLOYEE
WHERE LNAME=’Brown’;

DELETE FROM EMPLOYEE
WHERE SSN=’123456789’;

DELETE FROM WHERE
EMPLOYEE DNO IN
             (SELECT DNUMBER
             FROM DEPARTMENT
             WHERE DNAME=’Research’);
             
DELETE FROM EMPLOYEE;
~~~~


## 3.3 UPDATE
### UPDATE Statement
+ Syntax
~~~
UPDATE table_name
  SET {column_name = expression | NULL |
    DEFAULT | single_row_column select statement
    }[,...]
  [ WHERE condition ]
~~~
+ expression: An expression to assign to the column. The expression may use the old values of this and other columns in the table
+ DEFAULT: Set the column to its default value
+ NULL: Set the column to NULL
+ **single_row_column select statement**: a query returns one column and no more than one row. If no row is returned, NULL value is used
+ **condition**: An expression that returns a value of type boolean. Only rows for which this expression returns true will be updated.

### Semantics
+ Each UPDATE command modifies tuples in the same relation;
+ UPDATE changes the values of the specified columns in all rows that satisfy the condition;
+ Only the columns to be modified need be mentioned in the statement;
+ Columns not explicitly SET retain their previous values;
+ Updating table rows obeys rules and restrictions similar to ones with INSERT statement;
+ All column values have to be of the same or compatible data types and sizes with corresponding column definitions;
+ If a constraint specified with ON UPDATE CASCADE or ON UPDATE SET NULL is violated, target table and child table’s columns are updated with the corresponding values.

### Examples
+ Change the location and controlling department number of project number 10 to ’Bellaire’ and 5, respectively
~~~
UPDATE PROJECT
SET PLOCATION = ’Bellaire’,
    DNUM = 5
WHERE PNUMBER=10
~~~

+ Give all employees in the ’Research’ department a 10% raise in salary.
~~~
UPDATE EMPLOYEE
SET SALARY = SALARY *1.1
WHERE DNO IN ( SELECT DNUMBER
              FROM DEPARTMENT
              WHERE DNAME=’Research’)
~~~

  - In this request, the modified SALARY value depends on the original SALARY value in each tuple
  - The reference to the SALARY attribute on the right of = refers to the old SALARY value before modification
  - The reference to the SALARY attribute on the left of = refers to the new SALARY value after modification

### UPDATE Statement: University Database
+ The following scripts (for Oracle only) show:
  - how to create tables;
  - different usages of ALTER TABLE statement
  - different usages of INSERT statement
  - different usages of UPDATE statement
  - different usages of DELETE statement
+ [Click here for the code to create a simple UNIVERSITY relational database.](../Resources/UniversitySchema.sql)
+ [Click here for the code to drop all tables of the UNIVERSITY relational database.](../Resources/UniversityClear.sql)


# 4.Data Query Language
## SELECT 
### SELECT Statement: Overview
+ SQL has one basic statement for retrieving information from a database – the SELECT statement
  - This is not the same as the SELECT operation of the relational algebra
+ Important distinction between SQL and the formal relational model
  - SQL allows a table (relation) to have two or more tuples that are identical in all their attribute values
  - Hence, an SQL relation (table) is a multi-set (sometimes called a bag) of tuples; it is not a set of tuples
  
+ SQL relations can be constrained to be sets by specifying PRIMARY KEY or UNIQUE attributes, or by using the DISTINCT option in a query
### SELECT Statement: Concepts
+ A **bag** or **multi-set** is like a set, but an element may appear more than once
  - Example: {A, B, C, A} is a bag. {A, B, C} is also a bag that also is a set.
  - Bags also resemble lists, but the order is irrelevant in a bag.
+ Example:
  - {A, B, A} = {B, A, A} as bags
  - However, [A, B, A] is not equal to [B, A, A] as lists
  

## One Table Query
### Single Table SELECT Statement: Syntax
+ Syntax
~~~
SELECT [DISTINCT] { [<qualifier>.]column_name |
                      *|
                      expression [[AS] column_alias]
                      } [,...]
FROM  table_or_view_name [[AS] table_alias]
[WHERE condition]
[GROUPBY {[qualifier.]<column_name}[,...]
  [HAVING condition]
]
[ORDER BY {column_name | column_number [ASC | DESC]} [,...] ];
~~~

#### Single Table SELECT Statement: SELECT Clause
+ The SELECT list (between the key words SELECT and FROM) specifies expressions that form the output rows of the SELECT statement.
  - The expressions can refer to columns of the table specified in FROM clause.
  - Using the clause **AS output_name**, another name can be specified for an output column. This name is primarily used to label the column for display. It can also be used to refer to the column’s value in ORDER BY and GROUP BY clauses, but not in the WHERE or HAVING clauses; there you must write out the expression instead.
  - Instead of an expression, * can be written in the output list as a shorthand for all the columns of the selected rows.
  - **DISTINCT** is optional, used to eliminate duplicate rows.
#### Single Table SELECT Statement: FROM Clause
+ The FROM clause specifies one source tables for the SELECT.
  - Each table can have an alias.
  - The table alias can be used just like the table name.
  - The alias is necessary when we want to join a table with itself
  - Oracle doesn’t support **AS** when specifying the table alias
#### Single Table SELECT Statement: WHERE Clause
+ WHERE condition
  - where condition is any expression that evaluates to a result of type boolean. Any row that does not satisfy this condition will be eliminated from the output.
  - A row satisfies the condition if it returns true when the actual row values are substituted for any variable references.
#### Single Table SELECT Statement: GROUP BY Clause
+ GROUP BY expression
  - GROUP BY will condense into a single row all selected rows that share the same values for the grouped expressions.
  - Expression can be an input column name, or the name or ordinal number of an output column (SELECT list item), or an arbitrary expression formed from input-column values.
  - In case of ambiguity, a GROUP BY name will be interpreted as an input-column name rather than an output column name.
  - Aggregate functions, if any are used, are computed across all rows making up each group, producing a separate value for each group (whereas without GROUP BY, an aggregate produces a single value computed across all the selected rows).
  - When GROUP BY is present, it is not valid for the SELECT list expressions to refer to ungrouped columns except within aggregate functions, since there would be more than one possible value to return for an ungrouped column.
  
#### Single Table SELECT Statement: HAVING Clause
+ HAVING condition
  - HAVING eliminates group rows that do not satisfy the condition.
  - HAVING is different from WHERE: WHERE filters individual rows before the application of GROUP BY, while HAVING filters group rows created by GROUP BY.
  - Each column referenced in condition must unambiguously reference a grouping column, unless the reference appears within an aggregate function.
#### Single Table SELECT Statement: ORDER BY Clause
+ ORDER BY expression [ASC|DESC] [,...]
  - expression can be the name or ordinal number of an output column (SELECT list item), or it can be an arbitrary expression formed from input-column values.
  - The ORDER BY clause causes the result rows to be sorted according to the specified expressions. If two rows are equal according to the leftmost expression, the are compared according to the next expression and so on.
  - If they are equal according to all specified expressions, they are returned in an implementation-dependent order.
  - It is also possible to use arbitrary expressions in the ORDER BY clause, including columns that do not appear in the SELECT result list.
  - Optionally one may add the key word ASC (ascending) or DESC (descending) after any expression in the ORDER BY clause. If not specified, ASC is assumed by default.
  - The null value sorts higher than any other value. In other words, with ascending sort order, null values sort at the end, and with descending sort order, null values sort at the beginning.
  
#### Single Table SELECT Statement: Processing Sequence
+ 1. All rows of the table specified in the **FROM** clause are computed.
+ 2. If the **WHERE** clause is specified, all rows that do not satisfy the condition are eliminated from the output.
+ 3. If the **GROUP BY** clause is specified, the output is divided into groups of rows that match on one or more values. If the HAVING clause is present, it eliminates groups that do not satisfy the given condition.
+ 4. The actual output rows are computed using the **SELECT** output expressions for each selected row.
+ 5. If the **ORDER BY** clause is specified, the returned rows are sorted in the specified order. If ORDER BY is not given, the rows are returned in whatever order the system finds fastest to produce.
+ **DISTINCT** eliminates duplicate rows from the result. DISTINCT ON eliminates rows that match on all the specified expressions. ALL (the default) will return all candidate rows, including duplicates.

#### Single Table SELECT Statement: Examples
+ Retrieve the birthdate and address of the employee whose name is ’John B. Smith’.
~~~~
SELECT BDATE, ADDRESS 
FROM EMPLOYEE
WHERE FNAME=’John’ AND MINIT=’B’ AND LNAME=’Smith’
~~~~
+ Retrieve all rows of the relation WORKS_ON ordered by project no.
~~~~
SELECT *
FROM WORKS_ON
ORDER BY PNO
~~~~
+ Retrieve SSN of employees who are working on a project.
~~~~
SELECT DISTINCT ESSN
FROM WORKS_ON
~~~~
+ Retrieve the names of all employees who do not have supervisors
~~~~
SELECT FROM WHERE
FNAME, LNAME
WHERE SUPER_SSN IS NULL
~~~~
  - SQL allows queries that check if a value is NULL (missing or undefined or not applicable)
  - SQL uses IS or IS NOT to compare NULLs because it considers each NULL value distinct from other NULL values, so equality comparison is not appropriate.
+ The column can be identified uniquely by the table name and column name in the SQL in the following format:
~~~~
TableName.ColumnName
where TableName can be the name or alias of a table.
~~~~
+ To declare an alias of a table, using the following format in the FROM clause:
~~~~
FROM table_name table_alias
~~~~
+ For each employee, retrieve the employee’s name working for the department 5 and his supervisor is ’888665555’.
~~~~
SELECT FNAME, LNAME
FROM EMPOLYEE E
WHERE E.DNO = 5 AND E.SUPER_SSN = ’888665555’
~~~~
  - The alias is no necessary in the above example, which is used only to show the syntax and usage of aliases.
+ Retrieve Ssn, Salary, and Bonus for each employee, where Bonus is 20% of the Salary.
~~~~
SELECT SSN, Salary Salary*0.2 As Bonus 
FROM EMPLOYEE
ORDER BY DNO, SALARY
~~~~
+ Retrieve the number of employees for each department
~~~~
SELECT DNO, COUNT(*) AS EmployeeNum 
FROM EMPLOYEE
GROUP BY DNO
~~~~
  - Common aggregate functions: COUNT, AVG, SUM, MIN, MAX,
+ Retrieve the department such that its average salary is greater than $25000.
~~~~
SELECT DNO, AVG(SALARY) AS AverageSalary
FROM EMPLOYEE
GROUP BY DNO Having AverageSalary > 25000
~~~~
### Single Table SELECT Statement: Logical Operators
#### Logical operators supported in SQL
|Operator|Action|
|----|----|
|AL|TRUE if all of a set of comparisons are TRUE|
|AND|TRUE if both Boolean expressions are TRUE|
|ANY|TRUE if any one of a set of comparisons are TRUE|
|BETWEEN|TRUE if the operand is within a range|
|EXISTS|TRUE if a subquery contains any rows|
|IN|TRUE if the operand is equal to one of a list of expressions|
|LIKE|TRUE if the operand matches a pattern|
|NOT|Reserves the value of any other Boolean operator|
|OR|TRUE if either Boolean expression is TRUE|
|SOME|TRUE if some of a set of comparison are TRUE|

#### Examples
+ Retrieve SSN of employees who has no supervisor.
~~~~
SELECT SSN
FROM EMPLOYEE
WHERE Super_ssn IS NULL
~~~~
+ Retrieve SSN of employees who has supervisors
~~~
SELECT SSN
FROM EMPLOYEE
WHERE Super_ssn IS NOT NULL
~~~
+ Retrieve Ssn and address of employees who have one or more dependents
~~~
SELECT SSN,ADDRESS
FROM EMPLOYEE
WHERE SSN IN(SELECT DISTINCT ESSN
          FROM DEPENDENT)
~~~
+ Retrieve Ssn of employees whose first names begin with ’A’
~~~
SELECT  SSN
FROM EMPLOYEE
WHERE Fname LIKE ’A%’
~~~
+ Retrieve Ssn of employees whose first names doesn’t begin with ’A’ and ’B’
~~~~
SELECT  SSN
FROM EMPLOYEE
WHERE Fname NOT LIKE ’A%’
~~~~
  - % : Matches any string of zero or more characters
  - _: matches any single character within a string
+ Retrieve Ssn of employees whose salary is between$30000 and $50000
~~~~
SELECT SSN
FROM  EMPLOYEE
WHERE SALARY BETWEEN 30000 AND 50000
~~~~
+ Retrieve Ssn of employees who works on one of the project employee with ssn 123456789 is involved.
~~~~
SELECT SSN
FROM  WORKS_ON
WHERE PNO IN(SELECT PNO
            FROM WORKS_ON
            WHERE ESSN='123456789'
~~~~
+ Retrieve Ssn of employees who works on project 1, 2, or 3 using **explicit (enumerated) set of values**.
~~~~
SELECT SSN
FROM  WORKS_ON
WHERE PNO IN (1,2,3)
~~~~
+ Retrieve Ssn, Salary, Dno of employees whose salary is greater than at least one employee of department #5.
~~~~
SELECT SSN
FROM EMPLOYEE
WHERE SALARY > ANY(SELECT SALARY
                  FROM EMPLOYEE
                  WHERE DNO = 5)
~~~~
+ Retrieve Ssn, Salary, Dno of employees whose salary is greater than all employee of department #5.
~~~~
SELECT SSN
FROM EMPLOYEE
WHERE SALARY > ALL(SELECT SALARY
                  FROM EMPLOYEE
                  WHERE DNO = 5)
~~~~
+ **ANY** and **SOME** are interchangeable.

+ Retrieve Ssn, Address of employees who have dependents
~~~~
SELECT SSN,ADDRESS
FROM  EMPLOYEE
WHERE EXISTS(SELECT *
            FROM DEPENDENT
            WHERE ESSN=SSN)
~~~~

+ Retrieve Ssn, Address of employees who have NO dependents
~~~~
SELECT SSN,ADDRESS
FROM  EMPLOYEE
WHERE NOT EXISTS(SELECT *
            FROM DEPENDENT
            WHERE ESSN=SSN)
~~~~
### Precedence Order
+ In SQL, the precedence order from highest to lowest is as follows
|Precedence|Operator|
|---|---|
|Highest|Comparison Operators|
||NOT|
||AND|
|Lowest|OR|
+ Examples: Compare the output of the following two queries.
~~~~
SELECT SSN, ADDRESS
FROM EMPLOYEE
WHERE Sex = ’M’ OR Salary > 25000 AND Dno > 4



SELECT SSN, ADDRESS
FROM EMPLOYEE
WHERE (Sex = ’M’ OR Salary > 25000) AND Dno > 4
~~~~
## Combining Queries
## Multi-table Query
### INNER JOIN
### OUTER JOIN
# 5.SQL Function
# 6.Catalog & Schema
