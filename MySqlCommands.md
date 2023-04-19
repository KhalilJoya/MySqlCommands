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
SELECT Course.CourseName, SUM(amount) AS totalamount FROM studentCourse
JOIN Fees ON Fees.studentId = studentCourse.studentId
JOIN course ON course.courseId = studentCourse.courseId
GROUP BY course.courseName ORDER BY totalamount desc limit 1;
```

### 13. Vo course btana hai jisme sbse kam income hui hai

```sql
SELECT Course.CourseName, SUM(amount) AS totalamount FROM studentCourse
JOIN Fees ON Fees.studentId = studentCourse.studentId
JOIN course ON course.courseId = studentCourse.courseId
GROUP BY course.courseName ORDER BY totalamount limit 1;
```

### 14. Kaunsa course hai jisme sbse jyada students hai

```sql
SELECT Course.CourseName, COUNT(student.studentId) AS totalstudent FROM student
JOIN studentCourse ON student.studentId = studentCourse.studentId
JOIN course ON course.courseId = studentCourse.courseId GROUP BY course.courseName ORDER BY totalstudent DESC LIMIT 1;
```

### 15. Kaunsa course hai jisme sbse kam students hain

```sql
SELECT Course.CourseName, COUNT(student.studentId) AS totalstudent FROM student
JOIN studentCourse ON student.studentId = studentCourse.studentId
JOIN course ON course.courseId = studentCourse.courseId GROUP BY course.courseName ORDER BY totalstudent LIMIT 1;
```

### 16. Kaunsa course hai jisme koi admission ni hua hai

```sql
SELECT courseName FROM course WHERE courseId NOT IN(SELECT courseId FROM studentCourse);
```

### 17. Sbse jyada expense from expense table kis chiz ke liye hua hai

```sql
SELECT Expense_Details, SUM(amount) AS maximum_Expense FROM expenses GROUP BY Expense_Details ORDER BY maximum_Expense DESC LIMIT 1;
```

### 18. Employee list print krvani hai unki total paid salary ke descending order me

- Sajid Teacher 100000
- Shahrukh Teacher 80000
- Rehna Peon 20000

```sql
SELECT employee.Name, employee.Work, SUM(amount) AS total_salary
FROM employee LEFT JOIN salary ON salary.EmployeeTypeiddd = employee.employeeId
LEFT JOIN employeeType ON employee.employeeId = employeeType.employeeTypeId
GROUP BY employee.Name, employee.Work ORDER BY total_salary DESC;
```

### 19. Course table ko alter krna hai aur usme ek teacherid column add krna hai jo ki foreign key hogi

```sql
ALTER TABLE course ADD COLUMN teacherId INTEGER UNSIGNED;
ALTER TABLE course ADD FOREIGN KEY(teacherId) REFERENCES student(studentId);
```

### 20. Kaunse course me kaunsa teacher pdhata hai uski detail print krvani hai CourseName CourseTime TeacherName

```sql
SELECT * FROM course;

UPDATE course SET teacherId = 1 WHERE courseId = 1;
UPDATE course SET teacherId = 1 WHERE courseId = 2;
UPDATE course SET teacherId = 1 WHERE courseId = 3;
UPDATE course SET teacherId = 2 WHERE courseId = 4;

SELECT courseName, classTime, Name FROM course 	JOIN employee ON course.teacherId = employee.employeeId;
```

### 21. Student list print krvani hai unki total paid fees ke descending order me

- Sajid Nodejs 100000
- Shahrukh JavaScript 80000
- Rehna HTML 20000

```sql
SELECT student.studentName, course.courseName, SUM(amount) AS total_fees FROM student
JOIN studentCourse ON  student.studentId = studentCourse.studentId
JOIN Fees ON student.studentId = Fees.studentId
JOIN course ON course.courseId = studentCourse.courseId
GROUP BY student.studentName, course.courseName ORDER BY total_fees DESC;
```

### 22. Kisi b year ka total profit/loss btana hai Total fees - (total salary + total expenses)

```sql
SELECT (SELECT SUM(amount) FROM Fees WHERE YEAR(months)) - ((SELECT SUM(amount) FROM salary WHERE
 YEAR(months)) + (SELECT SUM(amount) FROM expenses WHERE YEAR(Date))) AS total_amount;
```

### 23. Student count list print krvani hai unki total batch count ke descending order me

- Nodejs 10:00PM 10/01/2023 10/07/2023 30
- JS 9:00PM 10/01/2023 10/07/2023 20
- HTML 10:00AM 10/01/2023 10/07/2023 40

```sql
SELECT course.courseName, course.classTime, course.coursestartDate, course.courseendDate, COUNT(student.studentId) AS total_student FROM course
JOIN studentCourse ON studentCourse.courseId = course.courseId
JOIN student ON student.studentId = studentCourse.studentId
GROUP BY course.courseName, course.classTime, course.coursestartDate, course.courseendDate ORDER BY total_student DESC;
```

### 24. Student list print krvani hai unki total student in pincode ke descending order me

- 302012 Jhotwara 20
- 303012 Merta 15
- 304013 Sikar 12

```sql
SELECT address.pincode, address.colony, COUNT(address.pincode) AS total_student FROM address JOIN student
ON student.studentId = address.studentId GROUP BY address.pincode, address.colony ORDER BY total_student DESC;
```

### 25. Ek view bnana hai.

- StudentName Mobie Email CourserName CourseStartDate CourseEndDate Time TotalFees AvgMarks
- Address(plotno, colony, city, state, pincode)

```sql
CREATE VIEW studentRecord AS
SELECT s.studentName, s.mobileNumber, s.email_Address, c.courseName, c.coursestartDate, c.courseendDate, c.classTime,
SUM(f.amount) AS totalFees, AVG(r.obtainedMarks) AS AvgMarks, CONCAT(a.plotNo, ' ', a.colony, ' ', a.city,' ', a.state, ' ', a.pincode) AS address
FROM student s LEFT JOIN studentCourse sc ON s.studentId = sc.studentId
LEFT JOIN course c ON c.courseId = sc.courseId
LEFT JOIN address a ON a.studentId = s.studentId
LEFT JOIN Fees f ON f.studentId = s.studentId
LEFT JOIN result r ON r.studentId = s.studentId
GROUP BY  s.studentName, s.mobileNumber, s.email_Address, c.courseName, c.coursestartDate, c.courseendDate, c.classTime,
f.amount, address;

select * from studentRecord
```

### 26. Ek view result pr bnana hai

- StudentName Mobile TotalMakrs TotalOBtainedMarks Avg Rank
- Sajid 945655445 500 400 80 1

```sql
CREATE VIEW studentResultTable AS
SELECT s.studentName, s.mobileNumber, r.totalMarks, AVG(r.obtainedMarks) AS AvgMarks
FROM student s
LEFT JOIN result r ON s.studentId = r.studentId
GROUP BY s.studentName, s.mobileNumber, r.totalMarks ORDER BY AvgMarks DESC;

select * from studentResultTable;

SELECT *, ROW_NUMBER() OVER(order by AvgMarks) FROM studentResultTable;
```

### 27. Vo student jinhone koi b fees jama ni krayi hai unko delete kr dena hai student table se Aur iske child records Address, Result tables me hai vo vha se b delete krna hai

```sql
SET SQL_SAFE_UPDATES = 0;

SET FOREIGN_KEY_CHECKS = 0;

DELETE FROM student WHERE studentId NOT IN(SELECT studentId FROM Fees);

SET FOREIGN_KEY_CHECKS = 1;
```

### 28. Expenses table se monthly expenses descending order me btane hai

- Year Month TotalExpenses
- 2023 Feb 25000
- 2022 Dec 12000

```sql
SELECT YEAR(expenses.Date) AS Year, MONTH(expenses.Date) AS month,
SUM(amount) AS total_expenses from expenses GROUP BY Year,Month;
```

### 29. Kaunse teacher ke batch ke students ki performance sbse best hai

```sql
SELECT e.Name AS teacherName, s.studentName, c.courseName, AVG(r.obtainedMarks) AS marks
FROM course c JOIN employee e ON c.teacherId = e.employeeId
JOIN studentCourse sc ON sc.courseId = c.courseId
JOIN student s ON s.studentId = sc.studentId
JOIN result r ON r.studentId = s.studentId
GROUP BY teacherName, s.studentName, c.courseName ORDER BY marks DESC LIMIT 1;
```

### 30. ek view bnana hai TeacherName BatchName BatchStartDate BatchEndDate Designation TotalFeesDeposit By ThisBatch TotalSalary

```sql
CREATE VIEW teacherRecord AS
SELECT e.Name, c.courseName, c.coursestartDate, c.courseendDate, e.Work,
SUM(f.amount) AS This_Batch, SUM(sa.amount) AS total_salary
FROM employee e JOIN course c ON c.teacherId = e.employeeId
JOIN studentCourse sc ON sc.courseId = c.courseId
JOIN student s ON s.studentId = sc.studentId
JOIN Fees f on f.studentId = s.studentId
JOIN salary sa ON sa.EmployeeTypeiddd = e.employeeId
GROUP BY e.Name, c.courseName, c.coursestartDate, c.courseendDate, e.Work;

select * from teacherRecord;
```
