# MySql

### create database

```sql
create database InstitueManagement;
```

### use

```sql
use InstitueManagement;
```

### Students Table

```sql
create table `Students`(
`Studentid` integer unsigned primary key not null auto_increment,
`Name` varchar(155) not null,
`FatherName` varchar(155) not null,
`MobileNumber` bigint not null,
`Email_Address` varchar(155) not null,
`Age` integer not null,
`Gander` varchar(155) not null,
`AdmissionDate` date not null
);
```

### Address Table

```sql
CREATE TABLE address(
`AddressId` INTEGER UNSIGNED PRIMARY KEY AUTO_INCREMENT,
`PlotNo` INTEGER NOT NULL,
`Colony` VARCHAR(155) NOT NULL,
`Pincode` BIGINT NOT NULL,
`City` VARCHAR(155) NOT NULL,
`State` VARCHAR(155) NOT NULL,
`ADID` INTEGER unsigned,
FOREIGN KEY (ADID) REFERENCES students(studentid)
);
```

### Course Table

```sql
create table `Course` (
`Courseid` integer unsigned primary key not null auto_increment ,
`SubjectName` varchar (155) not null ,
`SubjectFees` integer not null ,
`StartDate` date not null ,
`EndDate` date not null ,
`ClassTime` time not null ,
`COID` INTEGER unsigned,
FOREIGN KEY (COID) REFERENCES students(studentid)
)
```

### StudentCourse Table

```sql
create table `StudentCourse` (
`StudentCourseid`integer unsigned primary key not null auto_increment ,
`Studentid` integer not null,
`Courseid` integer not null ,
primary key (StudentCourseid,Studentid,Courseid)
);
```

### Fees Table

```sql
create table `Fees` (
`Feesid` integer unsigned primary key not null auto_increment ,
`Amount` bigint not null ,
`Months` varchar (155),
`FEID` INTEGER unsigned,
FOREIGN KEY (FEID) REFERENCES students(studentid)
)
```

### Result Table

```sql
create table `Result`(
`Resultid` integer unsigned primary key not null auto_increment ,
`TotalMarks` integer not null,
`ObtainedMarks` integer not null,
`ResultPassAndFail` varchar(155) not null,
`Studentid` integer not null,
`Testid` integer not null,
primary key (Resultid,Studentid,Testid)
)
```

### Test Table

```sql
create table `Test`(
`Testid` integer unsigned primary key not null auto_increment ,
`Name` varchar(155) not null,
`TotalMarks` integer not null,
`PassingMarks` integer not null,
`TestDate` date,
`Teid` integer unsigned,
 CONSTRAINT FK_Result_TestId FOREIGN KEY (Teid)
	REFERENCES Students(Studentid)
)
```

### Employee Table

```sql
create table `Employee`(
`Employeeid` integer unsigned primary key not null auto_increment ,
`Name` varchar (155) not null ,
`Work` varchar (155) not null,
`EmployeeTypeid` integer unsigned,
 CONSTRAINT FK_Result_EmployeeId FOREIGN KEY (EmployeeTypeid)
	REFERENCES EmployeeType(EmployeeTypeid)
);
```

### EmployeeType Table

```sql
create table `EmployeeType` (
`EmployeeTypeid` integer unsigned primary key not null auto_increment ,
`EmployeeType` varchar(155) not null
);
```

### Salary Table

```sql
create table `Salary`(
`Salaryid`  integer unsigned primary key not null auto_increment ,
`Amount` bigint not null,
`Months` varchar (155) not null,
`Employeeid` integer unsigned,
 CONSTRAINT FK_Result_SelaryId FOREIGN KEY (Employeeid)
	REFERENCES Employee(Employeeid)
)
```

### Expenses Table

```sql
create table `Expenses` (
`Expensesid`  integer unsigned primary key not null auto_increment ,
`Expense Details` varchar (155) not null,
`Amount` bigint not null,
`Date` date
);
```

### All Question queries

### 1. Name of all students jinhone abi tak koi fees deposit ni ki

```sql
select * from student left join fees on student.studentid = fees.feesid where amount = null;
select * from student where studentid not in (select studentid from fees);
```

### 2. Sbse jyada fees kis student ne di hai. naam btao

```sql
SELECT studentName, SUM(amount) AS total FROM student JOIN Fees ON student.studentId = Fees.studentId GROUP BY studentName ORDER  BY total DESC LIMIT 1;
```

### 3. Sbse jyada fees me 2nd number pr jo student hai uska naam btao

```sql

```

### 4. Coaching me total kitne employees hain unka naam and designation/role show krvao

```sql
SELECT COUNT(*), Name, Work FROM employee GROUP BY Name, Work;
```

### 5. Kis employee ne kitni salary uthayi hai ab tak vo btao ya month wise

```sql
SELECT Name, SUM(amount) FROM employee JOIN salary ON employee.employeeId = salary.employeeId GROUP BY Name;
```

### 6. Vo sare students jo abi tak 1 b test me fail hue hai unka naam, subject, totalmarks, passingmarks, obtainedmarks btao

```sql
SELECT student.studentName, test.subjectName, result.totalMarks, test.passingmarks, result.obtainedmarks, result.resultShow FROM student JOIN result ON student.studentId = result.studentId  JOIN test ON student.studentId = test.studentId  AND test.passingmarks > result.obtainedmarks;
```

### 7. Particular month ka kharcha btao. Kharche me tume salary and expenses dono add krne hai

```sql
SELECT sum(salary.amount) as salray, sum(expense.amount) as expense, sum(salary.amount) + sum(expense.amount) AS
Total FROM salary LEFT JOIN expense ON salaryId = expenseId;
```

### 8. Particular month me total income btao

```sql
SELECT SUM(amount) FROM Fees WHERE month = '2023-01-31'
```

### 9. Total income aaj tak

```sql
SELECT SUM(amount) FROM Fees;
```

### 10. Total kharche aaj tak

```sql
SELECT SUM(amount) FROM expense;
```

### 11. Kis course me kitne students hai vo btane hai

```sql
SELECT course.courseName, course.coursestartDate, course.classTime, COUNT(course.courseName)
FROM student JOIN studentCourse ON student.studentId = studentCourse.studentId
JOIN course ON studentCourse.courseId = course.courseId
GROUP BY course.courseName, course.coursestartDate, course.classTime;
```

### 12. Vo course btana hai jisme sbse jyada income hui hai

### 13. Vo course btana hai jisme sbse kam income hui hai

### 14. Kaunsa course hai jisme sbse jyada students hai

### 15. Kaunsa course hai jisme sbse kam students hain
