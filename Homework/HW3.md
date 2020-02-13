# CSCI 360 Spring 2020
# Homework 3
# start: 02/04/2020
# due: 11:59pm 02/11/2020

# Q1: Suppose each of the following Update operations is applied directly to the database shown below. Discuss all integrity constraints violated by each operation, if any. Each operation is isolated from other operations. (5 points each. Total 55)

**Hint**: [Steps of checking constraint voilation](https://github.com/ZhangNingSAU/Spring-2020-CSCI-360-Database-Mgmt-Systems/blob/master/Resources/3.Steps%20of%20checking%20constraint%20violation.pdf)

![Q1](https://d2vlcm61l7u1fs.cloudfront.net/media%2F605%2F6052c100-6ddb-4570-8c74-bff61d413d87%2FphpDnEFxK.png)

+ a) Insert < 'Robert', 'F', 'Scott', '943775543', '21-JUN-42', '2365 Newcastle Rd, Bellaire, TX', M, 58000, '888665555', 1 > into EMPLOYEE.
+ b) Insert < 'ProductA', 4, 'Bellaire', 2 > into PROJECT.
+ c) Insert < 'Production', 4, '943775543', '01-OCT-88' > into DEPARTMENT.
+ d) Insert < '677678989', null, '40.0' > into WORKS_ON.
+ e) Insert < '453453453', 'John', M, '12-DEC-60', 'SPOUSE' > into DEPENDENT.
+ f) Delete the WORKS_ON tuples with ESSN= '333445555'.
+ g) Delete the EMPLOYEE tuple with SSN= '987654321'.
+ h) Delete the PROJECT tuple with PNAME= 'ProductX'.
+ i) Modify the MGRSSN and MGRSTARTDATE of the DEPARTMENT tuple with DNUMBER=5 to '123456789' and '01-OCT-88', respectively.
+ j) Modify the SUPERSSN attribute of the EMPLOYEE tuple with SSN= '999887777' to '943775543'.
+ k) Modify the HOURS attribute of the WORKS_ON tuple with ESSN= '999887777' and PNO= 10 to '5.0'.


# Q2: Consider the relation CLASS(Course#, Univ_Section#, InstructorName, Semester, BuildingCode, Room#, TimePeriod, Weekdays, CreditHours). This represents classes taught in a university with unique Univ_Section# per class. Give what you think should be various (at least two) candidate keys and write in your own words under what constraints each candidate key would be valid. (15 points)

# Q3:  Consider the following relations for a database that keeps track of student enrollment in courses and the books adopted for each course, specify all the foreign keys for this schema. (15 points)

~~~~
STUDENT (SSN, Name, Major, Bdate)
COURSE (Course#, Cname, Dept)
ENROLL (SSN, Course#, Quarter, Grade)
BOOK_ADOPTION (Course#, Quarter, Book_ISBN)
TEXT (Book_ISBN, Book_Title, Publisher, Author)
~~~~


# Q4:  Consider a STUDENT relation in a UNIVERSITY database with the following attributes (Name, SSN, Local_phone, Address, Cell_phone, Age, GPA). Note that the cell phone may be from a different city and state (or province) from the local phone. A possible tuple of the relation is shown below: (5 points each. Total 15)

|Name|SSN|LocalPhone|Address|Cellphone|Age|GPA|
|----|----|----|----|----|----|----|
|George Shaw William Edwards|123-45-6789|555-1234|123 Main St., Anytown, CA 94539|555-4321|19|3.75|

+ a) Identify the critical missing information from the LocalPhone and CellPhone attributes as shown in the example above. (Hint: How do call someone who lives in a different state or province?)  
+ b) Consider the Name attribute.  What are the advantages and disadvantages of splitting this field from one attribute into three attributes (first name, middle name, and last name)?
+ c) What general guideline would you recommend for deciding when to store information in a single attribute and when to split the information.


# Step 1: Answer Questions 1 - 4, save your answers in a PDF file, name it as "CSCI360_Homework3_JohnDoe(0123456).pdf", where 0123456 is your bee card number.

# Step 2: Submit your work on [Blackboard](https://blackboard.sau.edu/webapps/login/)
