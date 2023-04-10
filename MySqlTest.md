# MySQL

### 1. What is the query to get all the details of a student along with the fees and result data for the student?

```
select * from students s join fees f on s.studentId =f.SFID join result r on r.RFID = f.SFID;

```

### 2. How can we retrieve the data of a student who has paid fees for the month of January?

```
select * from students s join fees f on s.studentId =f.SFID where Months ='January';

```

### 3. What is the query to get the list of all students who have not yet paid their fees?

```
select * from students s join fees f on s.studentId =f.SFID where Amount = Null;

```

### 4. How can we fetch the list of students who scored more than 80 marks in any subject in the result table?

```
select * from students s join Result r on s.studentId =r.RFID where Obtainedmarks > 80;

```

### 5. What is the query to get the details of students along with the total amount of fees they have paid so far?

```
select sum(Amount) as total from students s join Fees f on s.studentId =f.SFID group by total;

```

### 6. How can we fetch the list of all students along with their fees data and result data, even if they do not have any data in the fees or result table?

```
select * from students s left join fees f on s.studentId =f.SFID  left join result r on f.SFID = r.RFID;

```

### 7. What is the query to get the name, email address, and mobile number of all the students who belong to the city of "Delhi"?

```
select name,mobile,pincode,Email_Address from students s join Fees f on s.studentId =f.SFID where city = 'Delhi' ;

```

### 8. How can we retrieve the details of students who belong to the city of "Mumbai" and have paid their fees for the month of February?

```
select * from students s join fees f on s.studentId =f.SFID where f.Months ='February' and s.city = 'Merta City';

```

### 9. What is the query to get the name, email address, and total amount of fees paid by all the students who have paid their fees?

```
select name,pincode,sum(Amount) as total from students s join Fees f on s.studentId =f.SFID group by name,pincode;

```

### 10. How can we fetch the details of all students who have taken any test before a specific date?

```
select * from students s join Result r on s.studentId =r.RFID where Testdate < 2023-03-29;

```

### 11. What is the query to get the name, email address, and subject name of all the students who have scored more than 90 marks in any subject?

```
select name ,Subject  from students s join Result r on s.studentId =r.RFID where Obtainedmarks > 90;

```

### 12. How can we retrieve the details of students who belong to the city of "Bangalore" and have scored less than 50 marks in any subject?

```
select * from students s join Result r on s.studentId =r.RFID where city = 'Bangalore' and Obtainedmarks < 50;

```

### 13. What is the query to get the details of all students along with their fees and result data, sorted by their name in ascending order?

```
select * from students s join fees f on s.studentId =f.SFID join result r on f.SFID = r.RFID order by name asc;

```

### 14. How can we fetch the name, email address, and total amount of fees paid by all the students who have not paid their fees yet?

```
select name , sum(amount) as total from students left join fees  on students.studentid not in (fees.SFID) group by name;

```

### 15. What is the query to get the details of all students who have paid their fees for the month of March and have scored more than 70 marks in any subject?

```
select * from students left join result on students.studentid in (result.ResultId);

```

### 16. How can we retrieve the details of students who have not taken any test yet?

```
select * from students left join result on students.studentid in (result.ResultId);

```

### 17. What is the query to get the name, email address, and subject name of all the students who have scored less than 40 marks in any subject?

```
select Name,mobile,Subject from students join result on students.studentid in (result.ResultId) and result.Obtainedmarks < 40;

```

### 18. How can we fetch the details of students who belong to the city of "Pune" and have paid their fees for the month of January?

```
select * from students join Fees on students.Studentid in(Fees.SFID) and students.city = 'Merta city' and Fees.months = 'February';

```

### 19 What is the query to get the name, email address, and mobile number of all the students who have not taken any test yet?

```
select Name,Mobile from students left join result on students.Studentid in (result.ResultId);

```

### 20 How can we retrieve the details of students who have scored more than 60 marks in all the subjects they have taken tests for?

```
select * from students join result on students.studentid in (result.ResultId) and result.Obtainedmarks > 60;

```
