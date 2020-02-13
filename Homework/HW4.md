# CSCI 360 Spring 2020
# Homework 4
# start: 02/13/2020
# due: 11:59pm 02/20/2020

# Q1: Given the following COMPANY relational database state, please display the result of each query specified by a relational algebra expression. (Total 64)

![company](https://d2vlcm61l7u1fs.cloudfront.net/media%2F500%2F500decc6-d150-4a9e-bca8-b1c219adb1de%2FphpahrxoM.png)

![state](https://d2vlcm61l7u1fs.cloudfront.net/media%2F605%2F6052c100-6ddb-4570-8c74-bff61d413d87%2FphpDnEFxK.png)


+ a) σ<sub>Dno = 5 AND Salary >= 25000</sub>(EMPLOYEE)
+ b) π<sub>ssn, Relationship</sub>(DEPENDENT)
+ c) π<sub>Ssn</sub>(EMPLOYEE) – PROJECT<sub>Essn</sub>(DEPENDENT)
+ d) π<sub>Mgr_ssn</sub>(DEPARTMENT) ∪ π<sub>ssn</sub>(DEPENDENT)

+ e) π<sub>Essn, Pno</sub>(WORKS_ON)  / π<sub>Pno</sub>( σ<sub>ssn = ‘123456789’</sub>(WORKS_ON))

+ f) π<sub>Super_ssn</sub>(EMPLOYEE)

+ g) ℑ<sub>AVERAGE(Salary)</sub>(EMPLOYEE)
+ h) <sub>Essn</sub>ℑ<sub>MAX(Hours)</sub>(WORKS_ON)
+ i) <sub>Dno, Sex</sub>ℑ<sub>COUNT(*)</sub>(EMPLOYEE)


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
