# MySql

## create database

```sql
create database InstitueManagement;
```

## use

```sql
use InstitueManagement;
```

## Students Table

```sql
create table `Student`(
`Studentid` integer unsigned primary key not null auto_increment,
`StudentName` varchar(155) not null,
`FatherName` varchar(155) not null,
`MobileNumber` bigint not null,
`Email_Address` varchar(155) not null,
`AdmissionDate` date not null
);
```

## Address Table

```sql
CREATE TABLE `Address`(
`AddressId` INTEGER UNSIGNED PRIMARY KEY AUTO_INCREMENT,
`PlotNo` INTEGER NOT NULL,
`Colony` VARCHAR(155) NOT NULL,
`Pincode` integer NOT NULL,
`City` VARCHAR(155) NOT NULL,
`State` VARCHAR(155) NOT NULL,
`StudentId` INTEGER unsigned unique,
FOREIGN KEY (StudentId) REFERENCES student(studentid)
);
```

## Course Table

```sql
create table `Course` (
`Courseid` integer unsigned primary key not null auto_increment ,
`CourseName` varchar (155) not null ,
`CourseFees` integer not null ,
`CourseStartDate` date not null ,
`CourseEndDate` date not null ,
`ClassTime` time not null
)
```

## StudentCourse Table

```sql
create table `StudentCourse` (
`Studentid` integer ,
`Courseid` integer ,
primary key (Studentid,Courseid)
);
```

## Fees Table

```sql
create table `Fees` (
`Feesid` integer unsigned primary key not null auto_increment ,
`Amount` integer not null ,
`Month` Date not null,
`StudentId` INTEGER unsigned,
FOREIGN KEY (StudentId) REFERENCES student(studentid)
)
```

## Result Table

```sql
create table `Result`(
`TotalMarks` integer not null,
`ObtainedMarks` integer not null,
`ResultShow` ENUM('Pass','Fail') not null,
`Studentid` integer not null,
`Testid` integer not null,
primary key (Studentid,Testid)
)
```

## Test Table

```sql
create table `Test`(
`Testid` integer unsigned primary key not null auto_increment ,
`SubjectName` varchar(155) not null,
`TotalMarks` integer not null,
`PassingMarks` integer not null,
`TestDate` date not null,
`StudentId` INTEGER unsigned unique,
FOREIGN KEY (StudentId) REFERENCES student(studentid)
)
```

## Employee Table

```sql
create table `Employee`(
`Employeeid` integer unsigned primary key not null auto_increment ,
`Name` varchar (155) not null ,
`Work` varchar (155) not null,
`StudentId` integer unsigned,
 CONSTRAINT FK_Result_EmployeeId FOREIGN KEY (StudentId)
	REFERENCES EmployeeType(EmployeeTypeid)
);
```

## EmployeeType Table

```sql
create table `EmployeeType` (
`EmployeeTypeid` integer unsigned primary key not null auto_increment ,
`EmployeeType` varchar(155) not null
);
```

## Salary Table

```sql
create table `Salary`(
`Salaryid`  integer unsigned primary key not null auto_increment ,
`Amount` integer not null,
`Months` date not null,
`EmployeeTypeid` integer unsigned,
 CONSTRAINT FK_Result_SelaryId FOREIGN KEY (EmployeeTypeid)
	REFERENCES Employee(Employeeid)
);
```

## Expenses Table

```sql
create table `Expenses` (
`Expensesid`  integer unsigned primary key not null auto_increment ,
`Expense_Details` varchar (155) not null,
`Amount` integer null,
`Date` date
);
```

## All Question queries

### 1. Name of all students jinhone abi tak koi fees deposit ni ki

```sql
select * from student left join fees on student.studentid = fees.feesid where amount = null;
select * from student where studentid not in (select studentid from fees);
```

### 2. Sbse jyada fees kis student ne di hai. naam btao

```sql
SELECT student.studentid,studentName, SUM(amount) AS total FROM student JOIN Fees ON student.studentId = Fees.studentId
GROUP BY student.studentid,studentName ORDER  BY total DESC LIMIT 1;
```

### 3. Sbse jyada fees me 2nd number pr jo student hai uska naam btao

```sql
SELECT student.studentid,studentName, SUM(amount) AS total FROM student JOIN Fees ON student.studentId = Fees.studentId
GROUP BY student.studentid,studentName ORDER  BY total DESC LIMIT 1,1;
```

### 4. Coaching me total kitne employees hain unka naam and designation/role show krvao

```sql
SELECT COUNT(*), Name, Work FROM employee GROUP BY Name, Work;
```

### 5. Kis employee ne kitni salary uthayi hai ab tak vo btao ya month wise

```sql
SELECT Name, SUM(amount) FROM employee JOIN salary ON employee.Employeeid = salary.EmployeeTypeid GROUP BY Name;
```

### 6. Vo sare students jo abi tak 1 b test me fail hue hai unka naam, subject, totalmarks, passingmarks, obtainedmarks btao

```sql
SELECT student.studentName, test.subjectName, result.TotalMarks, test.PassingMarks, result.ObtainedMarks, result.ResultShow FROM student JOIN result ON student.studentId = result.studentId  JOIN test ON student.studentId = test.studentId AND test.passingmarks > result.obtainedmarks;
```

### 7. Particular month ka kharcha btao. Kharche me tume salary and expenses dono add krne hai

```sql
SELECT (SELECT SUM(amount) FROM salary) + (SELECT SUM(amount) FROM expenses) as Total_amount;


SELECT sum(salary.amount) as salray, sum(expenses.amount) as expenses, sum(salary.amount) + sum(expenses.amount) AS
Total FROM salary right JOIN expenses ON salaryId = Expensesid;
```

### 8. Particular month me total income btao

```sql
SELECT SUM(amount) FROM Fees WHERE month = '2023-01-20';
```

### 9. Total income aaj tak

```sql
SELECT SUM(amount) FROM Fees;
```

### 10. Total kharche aaj tak

```sql
SELECT SUM(amount) FROM expenses;
```

### 11. Kis course me kitne students hai vo btane hai

```sql
SELECT Course.CourseName, Course.CourseStartDate, Course.ClassTime, COUNT(Course.CourseName)
FROM student JOIN StudentCourse ON student.Studentid = StudentCourse.Studentid
JOIN Course ON StudentCourse.Courseid = Course.Courseid
GROUP BY Course.CourseName, Course.CourseStartDate, Course.ClassTime;
```

### 12. Vo course btana hai jisme sbse jyada income hui hai

```sql

```

### 13. Vo course btana hai jisme sbse kam income hui hai

```sql

```

### 14. Kaunsa course hai jisme sbse jyada students hai

```sql

```

### 15. Kaunsa course hai jisme sbse kam students hain

```sql

```

### 16. Kaunsa course hai jisme koi admission ni hua hai

### 17. Sbse jyada expense from expense table kis chiz ke liye hua hai

### 18. Employee list print krvani hai unki total paid salary ke descending order me

### Sajid Teacher 100000

### Shahrukh Teacher 80000

### Rehna Peon 20000

### 19. Course table ko alter krna hai aur usme ek teacherid column add krna hai jo ki foreign key hogi

### 20. Kaunse course me kaunsa teacher pdhata hai uski detail print krvani hai CourseName CourseTime TeacherName

### 21. Student list print krvani hai unki total paid fees ke descending order me

### Sajid Nodejs 100000

### Shahrukh JavaScript 80000

### Rehna HTML 20000

### 22. Kisi b year ka total profit/loss btana hai Total fees - (total salary + total expenses)

### 23. Student count list print krvani hai unki total batch count ke descending order me

### Nodejs 10:00PM 10/01/2023 10/07/2023 30

### JS 9:00PM 10/01/2023 10/07/2023 20

### HTML 10:00AM 10/01/2023 10/07/2023 40

### 24. Student list print krvani hai unki total student in pincode ke descending order me

### 302012 Jhotwara 20

### 303012 Merta 15

### 304013 Sikar 12

### 25. Ek view bnana hai.

### StudentName Mobie Email CourserName CourseStartDate CourseEndDate Time TotalFees AvgMarks Address(plotno, colony, city, state, pincode)

### 26. Ek view result pr bnana hai

### StudentName Mobile TotalMakrs TotalOBtainedMarks Avg Rank

### Sajid 945655445 500 400 80 1

### 27. Vo student jinhone koi b fees jama ni krayi hai unko delete kr dena hai student table se Aur iske child records Address, Result tables me hai vo vha se b delete krna hai

### 28. Expenses table se monthly expenses descending order me btane hai

### Year Month TotalExpenses

### 2023 Feb 25000

### 2022 Dec 12000

### 29. Kaunse teacher ke batch ke students ki performance sbse best hai

### 30. ek view bnana hai TeacherName BatchName BatchStartDate BatchEndDate Designation TotalFeesDepositByThisBatch TotalSalary
