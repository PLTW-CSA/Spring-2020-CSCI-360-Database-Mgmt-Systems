# CSCI 360 Spring 2020
# Dr. Ning Zhang
# Topic 2: Database System Concepts and Architecture (Chapter 2)

# Outline
+ Data Models and Their Categories
+ History of Data Models
+ Schemas, Instances, and States
+ Three-Schema Architecture
+ Data Independence
+ DBMS Languages and Interfaces
+ Database System Utilities and Tools
+ Centralized and Client-Server Architectures
+ Classification of DBMSs

# 2.1 Data Models
+ Data Model:
  - A set of concepts to describe the structure of a database, the operations for manipulating these structures, and certain constraints that the database should obey.
+ Data Model Structure and Constraints:
  - Constructs are used to define the database structure
  - Constructs typically include elements (and their data types) as well as groups of elements (e.g. entity, record, table), and relationships among such groups
  - Constraints specify some restrictions on valid data; these constraints must be enforced at all times
+ Data Model Operations:
  - These operations are used for specifying database retrievals and updates by referring to the constructs of the data model.
  - Operations on the data model may include basic model operations (e.g. generic insert, delete, update) and user-defined operations (e.g. compute_student_gpa, update_inventory)



## 2.1.1 Categories of Data Models
+ Conceptual (high-level, semantic) data models:
  - Provide concepts that are close to the way many users perceive data. (Also called entity-based or object-based data models.)
    + entity (e.g. employee)
    + attribute (e.g. employee's name)
    + relationship (e.g. a work-on relationship between an employee and a project)
+ Physical (low-level, internal) data models:
  - Provide concepts that describe details of how data is stored in the computer. These are usually specified in an ad-hoc manner through DBMS design and administration manuals.
    + record formats
    + record orderings
    + access paths
+ Implementation (representational) data models:
  - Provide concepts that fall between the above two, used by many commercial DBMS implementations (e.g. relational data models used in many commercial systems).
+ Self-Describing Data Models:
  - Combine the description of data with the data values. Examples include XML, key-value stores and some NOSQL systems.

## 2.1.2 Schemas, Instances, and Database State
###  Schemas vs. Instances
+ Database Schema:
  - The description of a database.
  - Includes descriptions of the database structure, data types, and the constraints on the database.
+ Schema Diagram:
  - An illustrative display of (most aspects of) a database schema.
+ Schema Construct:
  - A component of the schema or an object within the schema, e.g., STUDENT, COURSE.
+ Database State:
  - The actual data stored in a database at a particular moment in time. This includes the collection of all the data in the database.
  - Also called database instance (or occurrence or snapshot).
    + The term instance  is also applied to individual database components, e.g. record instance, table instance, entity instance
### Database Schema vs. Database State
+ Database State: 
  - Refers to the content of a database at a moment in time.
+ Initial Database State:
  - Refers to the database state when it is initially loaded into the system.
+ Valid State:
  - A state that satisfies the structure and constraints of the database.
+ Distinction
  - The database schema changes very infrequently. 
  - The database state changes every time the database is updated. 
+ Meta-data
  - The description of the schema constructs and constraints.

+ Schema is also called intension.
+ State is also called extension.

### Example of a Database Schema


<img src="https://d2vlcm61l7u1fs.cloudfront.net/media%2Fd6c%2Fd6cbd079-6fbb-4d32-9efd-c04a59a7bb1a%2FphpOaAw7w.png" alt="Example of a Database Schema" width="800" height="600"/>

### Example of a database state

![Example of a database state](https://media.cheggcdn.com/media/c07/s684x874/c07635c4-72ce-41da-bdc1-965004793ac0/phpJzlBhp.png)

# 2.2 Three-Schema Architecture and Data Independence

## 2.2.1 Three-Schema Architecture  
+ Proposed to support DBMS characteristics of:
  - self-decribing
  - Program-data independence.
  - Support of multiple views of the data.
+ Not explicitly used in commercial DBMS products, but has been useful in explaining database system organization
+ Defines DBMS schemas at three levels:
  - Internal schema at the internal level to describe physical storage structures and access paths (e.g indexes). 
    + Typically uses a physical data model.
  - Conceptual schema at the conceptual level to describe the structure and constraints for the whole database for a community of users. 
    + Uses a conceptual or an implementation data model.
+ External schemas at the external level to describe the various user views. 
  - Usually uses the same data model as the conceptual schema.
  
  


<img src="https://i.stack.imgur.com/wrTfx.jpg" alt="Three-Schema Architecture" width="800" height="600"/>

+ Mappings among schema levels are needed to transform requests and data. 
  - Programs refer to an external schema, and are mapped by the DBMS to the internal schema for execution.
  - Data extracted from the internal DBMS level is reformatted to match the user’s external view (e.g. formatting the results of an SQL query for display in a Web page)
  
## 2.2.2 Data Independence
+ **Logical Data Independence**: 
  - The capacity to change the conceptual schema without having to change the external schemas and their associated application programs.
+ **Physical Data Independence**:
  - The capacity to change the internal schema without having to change the conceptual schema.
  - For example, the internal schema may be changed when certain file structures are reorganized or new indexes are created to improve database performance
+ When a schema at a lower level is changed, only the **mappings** between this schema and higher-level schemas need to be changed in a DBMS that fully supports data independence.
+ The higher-level schemas themselves are **unchanged**.
  - Hence, the application programs need not be changed since they refer to the external schemas.  
  
  
# 2.3 Database Languages and Interfaces
## 2.3.1 Database Languages
+ Data Definition Language (DDL)
+ Data Manipulation Language (DML)
  - High-Level or Non-procedural Languages: These include the relational language SQL
    + May be used in a standalone way or may be embedded in a programming language
  - Low Level or Procedural Languages:
    + These must be embedded in a programming language
### Data Definition Language (DDL)
+ Used by the DBA and database designers to specify the conceptual schema of a database.
+ In many DBMSs, the DDL is also used to define internal and external schemas (views).
+ In some DBMSs, separate storage definition language (SDL) and view definition language (VDL) are used to define internal and external schemas.
  - SDL is typically realized via DBMS commands provided to the DBA and database designers
### Data Manipulation Language (DML)
+ Used to specify database retrievals and updates
+ DML commands (data sublanguage) can be embedded in a general-purpose programming language (host language), such as COBOL, C, C++, or Java.
  - A library of functions can also be provided to access the DBMS from a programming language
+ Alternatively, stand-alone DML commands can be applied directly (called a query language).
+ Types of DML
  - **High Level or Non-procedural Language**:
    + For example, the SQL relational language
    + Are “set”-oriented and specify what data to retrieve rather than how to retrieve it. 
    + Also called declarative languages.
  - **Low Level or Procedural Language:**
    + Retrieve data one record-at-a-time; 
    + Constructs such as looping are needed to retrieve multiple records, along with positioning pointers.
## 2.3.2 Database Interfaces(DBMS Interfaces)
+ Stand-alone query language interfaces
  - Example: Entering SQL queries at the DBMS interactive SQL interface (e.g. SQL*Plus in ORACLE)
+ Programmer interfaces for embedding DML in programming languages
+ User-friendly interfaces
  - Menu-based, forms-based, graphics-based, etc.
+ Mobile Interfaces:interfaces allowing users to perform transactions using mobile apps
### DBMS Programming Language Interfaces
+ Programmer interfaces for embedding DML in a programming languages:
  - **Embedded Approach**: e.g embedded SQL (for C, C++, etc.), SQLJ (for Java)
+ **Procedure Call Approach**: e.g. JDBC for Java, ODBC (Open Databse Connectivity) for other programming languages as API’s (application programming interfaces)
+ **Database Programming Language Approach**: e.g. ORACLE has PL/SQL, a programming language based on SQL; language incorporates SQL and its data types as integral components
+ **Scripting Languages**: PHP (client-side scripting) and Python (server-side scripting) are used to write database programs.

### User-Friendly DBMS Interfaces
+ Menu-based (Web-based), popular for browsing on the web
+ Forms-based, designed for naïve users used to filling in entries on a form
+ Graphics-based 
  - Point and Click, Drag and Drop, etc.
  - Specifying a query on a schema diagram
+ Natural language: requests in written English
+ Combinations of the above:
  - For example, both menus and forms used extensively in Web database interfaces

### Other DBMS Interfaces
+ Natural language: free text as a query
+ Speech : Input query and Output response
+ Web Browser with keyword search
+ Parametric interfaces, e.g., bank tellers using function keys.
+ Interfaces for the DBA:
  - Creating user accounts, granting authorizations
  - Setting system parameters
  - Changing schemas or access paths


# 2.4 The database system enviroment

## 2.4.1 DBMS Component Modules

## 2.4.2 Database System Utilities

## 2.4.3 Tools, Application Enviroments, and Communication Facilities

# 2.5 Centralized and Client/Server Architectures for DMBSs
## 2.5.1 Centralized DBMSs Architecture
## 2.5.2 Basic Client/Server Architectures
## 2.5.3 Two-Tier Client/Server Architectures for dmbsS
## 2.5.4 Three-Tier and n-Tier Architectures for Web Applications
  
  
  
  
  
  
  
  
  
  
  
  
