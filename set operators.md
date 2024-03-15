Create table Students (Reg_no varchar NOT NULL, Name varchar, Age int, DOB date, PRIMARY KEY (Reg_no));
INSERT INTO Students (Reg_no, Name, Age, DOB) VALUES ('S001', 'John Smith', 20, '2003-May-10'), ('S002', 'Emily Johnson', 21, '2002-Sep-15'), ('S003', 'Michael Brown', 19, '2004-Feb-28'), ('S004', 'Sarah Davis', 22, '2000-Jul-20'), ('S005', 'James Wilson', 20, '2002-Apr-12'), ('S006', 'Emma Martinez', 21, '2001-Nov-28');

-- Create the Grades table with the desired structure CREATE TABLE Grades (Reg_no VARCHAR(10) NOT NULL, Subject VARCHAR(50) NOT NULL, Marks INT, PRIMARY KEY (Reg_no, Subject), FOREIGN KEY(Reg_no) REFERENCES Students(Reg_no));

-- Insert dummy values into Grades INSERT INTO Grades (Reg_no, Subject, Marks) VALUES ('S001', 'Mathematics', 85), ('S002', 'English', 78), ('S003', 'Science', 92), ('S004', 'Mathematics', 78), ('S004', 'English', 85), ('S004', 'Science', 92), ('S005', 'Mathematics', 70), ('S005', 'English', 82), ('S005', 'Science', 88), ('S006', 'Mathematics', 92), ('S006', 'English', 75), ('S006', 'Science', 80);

-- 1. Retrieve the names of students who have received grades in either Mathematics or English:
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Mathematics')
UNION
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'English');

-- 2. Retrieve the names of students who have received grades in both Mathematics and English:
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Mathematics')
INTERSECT
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'English');

-- 3. Retrieve the names of students who have received grades in Mathematics but not in English:
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Mathematics')
EXCEPT
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'English');

-- 4. Retrieve the names of students who have not received any grades in either Mathematics or English:
SELECT Name FROM Students WHERE Reg_no NOT IN (SELECT Reg_no FROM Grades WHERE Subject = 'Mathematics')
UNION
SELECT Name FROM Students WHERE Reg_no NOT IN (SELECT Reg_no FROM Grades WHERE Subject = 'English');

-- 5. Retrieve the names of students who have received grades in Mathematics or English, and also in Science:
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Mathematics')
UNION
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'English')
INTERSECT
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Science');

-- 6. Retrieve the names of students who have received grades in Mathematics or English, but not in Science:
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Mathematics'
UNION
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'English'))
EXCEPT
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Science');

-- 7. Retrieve the names of students who have received grades in all subjects:
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Mathematics')
INTERSECT
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'English')
INTERSECT
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Science');

-- 8. Retrieve the names of students who have received grades in at least one subject:
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades);

-- 9. Retrieve the names of students who have received grades in Mathematics but not in English or Science:
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Mathematics')
EXCEPT
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'English'
UNION
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Science'));

-- 10. Retrieve the names of students who have not received grades in any subject:
SELECT Name FROM Students WHERE Reg_no NOT IN (SELECT Reg_no FROM Grades);
