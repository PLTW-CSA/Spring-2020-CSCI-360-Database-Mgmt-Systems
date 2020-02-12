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
## Predefined Data Types: Character String Types
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
## Predefined Data Types: Binary String Types
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
## Predefined Data Types: Exact Numbers






# 2.Data Definition Language
## CREATE TABLE
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
