# MySql

### Students Table

```
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

```
create table `Address`(
`Addressid` integer unsigned primary key not null auto_increment,
`PlotNo.` integer not null ,
`Colony` varchar(155) not null ,
`Pincode` integer not null,
`City` varchar(155),
`State` varchar(155),
`Adsid` integer unsigned,
 CONSTRAINT FK_Result_AdId FOREIGN KEY (Adsid)
	REFERENCES Students(Studentid)
);

```

### Course Table

```
create table `Course` (
`Courseid` integer unsigned primary key not null auto_increment ,
`SubjectName` varchar (155) not null ,
`SubjectFees` integer not null ,
`StartDate` date not null ,
`EndDate` date not null ,
`ClassTime` time not null ,
`Couid` integer unsigned,
 CONSTRAINT FK_Result_CouId FOREIGN KEY (Couid)
	REFERENCES Students(Studentid)
)

```

### StudentCourse Table

```
create table `StudentCourse` (
`StudentCourseid`integer unsigned primary key not null auto_increment ,
`Studentid` integer not null,
`Courseid` integer not null ,
primary key (Studentid,Courseid)
)

```

### Fees Table

```
create table `Fees` (
`Feesid` integer unsigned primary key not null auto_increment ,
`Amount` bigint not null ,
`Months` varchar (155),
`Feid` integer unsigned,
 CONSTRAINT FK_Result_FeesId FOREIGN KEY (Feid)
	REFERENCES Students(Studentid)
)

```

### Result Table

```
create table `Result`(
`Resultid` integer unsigned primary key not null auto_increment ,
`TotalMarks` integer not null,
`ObtainedMarks` integer not null,
`ResultPassAndFail` varchar(155) not null,
`Studentid` integer not null,
`Testid` integer not null,
primary key (Studentid,Testid)
)

```

### Test Table

```
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

```
create table `Employee`(
`Employeeid` integer unsigned primary key not null auto_increment ,
`Name` varchar (155) not null ,
`Employeetype` varchar (155) not null,
`Work` varchar (155) not null,
`Emid` integer unsigned,
 CONSTRAINT FK_Result_EmployeeId FOREIGN KEY (Emid)
	REFERENCES Students(Studentid)
)

```

### EmployeeType Table

```
create table `EmployeeType` (
`EmployeeTypeid` integer unsigned primary key not null auto_increment ,
`EmployeeType` varchar(155) not null
)

```

### Salary Table

```
create table `Salary`(
`Salaryid`  integer unsigned primary key not null auto_increment ,
`Employeeid` integer not null,
`Amount` bigint not null,
`Months` varchar (155) not null,
`Salid` integer unsigned,
 CONSTRAINT FK_Result_SelaryId FOREIGN KEY (Salid)
	REFERENCES Employee(Employeeid)
)

```

### Expenses Table

```
create table `Expenses` (
`Expensesid`  integer unsigned primary key not null auto_increment ,
`Expense Details` varchar (155) not null,
`Amount` bigint not null,
`Date` date
)

```
