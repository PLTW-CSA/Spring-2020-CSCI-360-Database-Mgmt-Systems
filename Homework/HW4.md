# CSCI 360 Spring 2020
# Homework 4
# start: 02/13/2020
# due: 11:59pm 02/20/2020

# Q1: Given the following COMPANY relational database state, please display the result of each query specified by a relational algebra expression. (Total 64)

![company](https://d2vlcm61l7u1fs.cloudfront.net/media%2F500%2F500decc6-d150-4a9e-bca8-b1c219adb1de%2FphpahrxoM.png)

![state](https://d2vlcm61l7u1fs.cloudfront.net/media%2F605%2F6052c100-6ddb-4570-8c74-bff61d413d87%2FphpDnEFxK.png)


+ a) σ<sub>Dno = 5 AND Salary >= 25000</sub>(EMPLOYEE)
+ b) π<sub>Essn, Relationship</sub>(DEPENDENT)
+ c) π<sub>Ssn</sub>(EMPLOYEE) – PROJECT<sub>Essn</sub>(DEPENDENT)
+ d) π<sub>Mgr_ssn</sub>(DEPARTMENT) ∪ π<sub>Essn</sub>(DEPENDENT)

+ e) π<sub>Essn, Pno</sub>(WORKS_ON) ÷ π<sub>Pno</sub>( σ<sub>ssn = ‘123456789’</sub>(WORKS_ON))

+ f) π<sub>Super_ssn</sub>(EMPLOYEE)

+ g) ℑ<sub>AVERAGE(Salary)</sub>(EMPLOYEE)
+ h) <sub>Essn</sub>ℑ<sub>MAX(Hours)</sub>(WORKS_ON)
+ i) <sub>Dno, Sex</sub>ℑ<sub>COUNT(*)</sub>(EMPLOYEE)


# Q2: Consider the two tables T1 and T2 shown in the Figure below. Show the results of the following operations: (9 points each. Total 36 points)

## Table T1

|P|Q|R|
|----|----|----|
|10|A|5|
|15|B|8|
|25|A|6|


## Table T2

|A|B|C|
|----|----|----|
|10|B|6|
|25|C|3|
|10|B|5|


+ a). T1 ⋈ <sub>T1.P = T2.A</sub> T2
+ b). T1 ⟕ <sub>T1.P = T2.A</sub> T2
+ c). T1 ⟖ <sub>T1.Q = T2.B</sub> T2
+ d).T1 ⋈ <sub>T1.P = T2.A AND T1.R = T2.C</sub> T2





# Step 1: Answer Questions 1 - 2, save your answers in a PDF file, name it as "CSCI360_Homework4_JohnDoe(0123456).pdf", where 0123456 is your bee card number.

+ **Note**: You can use eiter operation symbols or names in your answer. For example, you can write one query as **SELECT<sub>Ssn</sub>(EMPLOYEEE)** or **σ<sub>Ssn</sub>(EMPLOYEEE)**. But you have to understand the meaning of the relational operation symbols once they are given, because I will use the symbols in  the homework assignments and exams.

# Step 2: Submit your work on [Blackboard](https://blackboard.sau.edu/webapps/login/)
