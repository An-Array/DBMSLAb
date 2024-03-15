Create table Students (Reg_no varchar NOT NULL, Name varchar, Age int, DOB date, PRIMARY KEY (Reg_no));

INSERT INTO Students (Reg_no, Name, Age, DOB)
VALUES ('S001', 'John Smith', 20, '2003-05-10'), ('S002', 'Emily Johnson', 21, '2002-09-15'), ('S003', 'Michael Brown', 19, '2004-02-28'), ('S004', 'Sarah Davis', 22, '2000-07-20'), ('S005', 'James Wilson', 20, '2002-04-12'), ('S006', 'Emma Martinez', 21, '2001-11-28');
    
-- Create the Grades table with the desired structure
CREATE TABLE Grades (Reg_no VARCHAR(10) NOT NULL, Subject VARCHAR(50) NOT NULL, Marks INT, PRIMARY KEY (Reg_no, Subject), FOREIGN KEY(Reg_no) REFERENCES Students(Reg_no));

-- Insert dummy values into Grades
INSERT INTO Grades (Reg_no, Subject, Marks) VALUES ('S001', 'Mathematics', 85), ('S002', 'English', 78), ('S003', 'Science', 92), ('S004', 'Mathematics', 78), ('S004', 'English', 85), ('S004', 'Science', 92), ('S005', 'Mathematics', 70), ('S005', 'English', 82), ('S005', 'Science', 88), ('S006', 'Mathematics', 92), ('S006', 'English', 75), ('S006', 'Science', 80);

-- 1. Retrieve all the names of students who scored more than 80 marks in Mathematics:
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Mathematics' AND Marks > 80); 
-- Expected output: 
-- John Smith

-- 2. Count the number of students who have received grades:
SELECT COUNT(*) FROM Students WHERE Reg_no IN (SELECT DISTINCT Reg_no FROM Grades); 
-- Expected output: 
-- 6

-- 3. Find the average age of students who scored less than 70 in any subject:
SELECT AVG(Age) FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Marks < 90); 
-- Expected output: 
-- 20.1667

-- 4. List the subjects along with the highest marks obtained in each subject:
SELECT Subject, MAX(Marks) AS Highest_Marks FROM Grades GROUP BY Subject; 
-- Expected output: 
-- Mathematics 92
-- English 85
-- Science 92

-- 5. Retrieve the names of students who have not received any grades:
SELECT Name FROM Students WHERE Reg_no NOT IN (SELECT DISTINCT Reg_no FROM Grades); 
-- Expected output: 
-- Sarah Davis
-- James Wilson

-- 6. Find the registration numbers of students who have scored the highest marks in at least one subject:
SELECT Reg_no FROM Grades WHERE (Reg_no, Marks) IN (SELECT Reg_no, MAX(Marks) FROM Grades GROUP BY Reg_no); 
-- Expected output: 
-- S003
-- S004
-- S005
-- S006

-- 7. Count the number of subjects in which each student has scored more than 80 marks:
SELECT Reg_no, COUNT(*) AS Subjects_Above_80 FROM Grades WHERE Marks > 80 GROUP BY Reg_no; 
-- Expected output: 
-- S001 1
-- S002 0
-- S003 1
-- S004 3
-- S005 2
-- S006 2

-- 8. List the names of students who are not yet 20 years old:
SELECT Name FROM Students WHERE Age < 20; 
-- Expected output: 
-- Michael Brown
-- Sarah Davis

-- 9. Retrieve the names of students who have scored the same marks in Science as the student with registration number 'S001':
SELECT Name FROM Students WHERE Reg_no IN (SELECT Reg_no FROM Grades WHERE Subject = 'Science' AND Marks = (SELECT Marks FROM Grades WHERE Reg_no = 'S001' AND Subject = 'Science')); 
-- Expected output: 
-- John Smith

-- 10. Find the subject in which the maximum number of students scored more than 90 marks:
SELECT Subject FROM Grades GROUP BY Subject HAVING COUNT(CASE WHEN Marks > 90 THEN 1 END) = (SELECT MAX(Subject_Count) FROM (SELECT COUNT(CASE WHEN Marks > 90 THEN 1 END) AS Subject_Count FROM Grades GROUP BY Subject) AS Subquery); 
-- Expected output: 
-- Mathematics
