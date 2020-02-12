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
  
  
## CREATE TABLE
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
|action|There are the following possible actions for each ON DELETE or ON UPDATE clause:
<ul>
  <li>NO ACTION: Produce an error indicating that the deletion or update would create a foreign key constraint violation. This is the default action.</li>
  <li>RESTRICT: Same as NO ACTION except that this action will not be deferred even if the rest of the constraint is deferrable and deferred.</li>
  <li>CASCADE: Delete any rows referencing the deleted row, or update the value of the referencing column to the new value of the referenced column, respectively.</li>
  <li>SET NULL: Set the referencing column values to null.</li>
  <li>SET DEFAULT: Set the referencing column values to their default value.</li>
  </ul>|
## CREATE VIEW
## DROP TABLE
## ALTER TABLE
# 3.Data Manipulation Language
## INSERT
## DELETE
## UPDATE
# 4.Data Query Language
## One Table Query
## Combining Queries
## Multi-table Query
### INNER JOIN
### OUTER JOIN
# 5.SQL Function
# 6.Catalog & Schema
