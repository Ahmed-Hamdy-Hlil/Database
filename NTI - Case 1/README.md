# ERD & Mapping

![NTI](https://github.com/Ahmed-Hamdy-Hlil/Images/blob/main/logo-1.png?raw=true)

NTI – Data Analysis

---

## Case 1

A big company has decided to store information about its projects and employees in a database. The company has wisely chosen to hire you as a database designer. Prepare an E-R diagram for this Company according to the following description:

### Description:

- The company has a number of employees. Each employee has:
  - **SSN**
  - **Birth Date**
  - **Gender**
  - **Name** (represented as **Fname** and **Lname**).

- The company has a set of departments. Each department has:
  - **DName**
  - **DNUM** (unique)
  - **Locations**.

- Employees work in several projects. Each project has:
  - **PName**
  - **PNumber** (identifier)
  - **Location**
  - **City**.

- Each employee may have a set of dependents. Each dependent has:
  - **Dependent Name** (unique)
  - **Gender**
  - **Birth Date**.

**Note:** If the employee leaves the company, there is no need to store information about their dependents.

- For each department:
  - There is always one employee assigned to manage that department.
  - Each manager has a **Hiring Date**.

- Relationships:
  - Department may have employees, but an employee must work in only one department.
  - Each department may have a set of projects, and each project must be assigned to one department.
  - Employees work on several projects, and each project has several employees.
  - Each employee has a number of working hours for each project.
  - Each employee has a supervisor.!
  - 
---
### Entities and Attributes
1. **Employee**
   - SSN
   - BirthDate
   - Gender
   - Fname
   - Lname

2. **Department**
   - DName
   - DNUM
   - Locations

3. **Project**
   - PName
   - PNumber
   - Location
   - City

4. **Dependent**
   - DependentName
   - Gender
   - BirthDate

---

### Relationships

1. **Manages:**
   - Employee manages one Department.
   - Attributes: HireDate.

2. **Works_In:**
   - Employee works in one Department.

3. **Has_Project:**
   - Department has many Projects.

4. **Works_On:**
   - Employee works on many Projects with NumberOfHours.

5. **Has_Dependent:**
   - Employee has many Dependents.

6. **Supervises:**
   - Employee supervises other Employees.

![ERD](https://github.com/Ahmed-Hamdy-Hlil/Images/blob/main/case%201_3-1.png?raw=true)

---
### Cardinalities

- **Manages:** 1:1 with Total Participation for both Employee and Department.
- **Works_In:** 1:N with Total Participation for Employee and Partial for Department.
- **Has_Project:** 1:N with Total Participation for Project and Partial for Department.
- **Works_On:** M:N with Partial Participation for both Employee and Project.
- **Has_Dependent:** 1:N with Partial Participation for Employee and Total for Dependent.
- **Supervises:** 1:N with Partial Participation for both Employee (supervisor and subordinate).

---

### Mapping
![Mapping](https://github.com/Ahmed-Hamdy-Hlil/Images/blob/main/case%201_6-1.png?raw=true)

![Mapping](https://github.com/Ahmed-Hamdy-Hlil/Images/blob/main/case%201_7-1.png?raw=true)

| Entity           | Attributes                       |
|------------------|----------------------------------|
| **EMP**          | SSN, Gender, BOD, F-Name, L-Name, D_Num, Sup_SSN |
| **Project**      | P-Number, City, Location, P-Name, D_Num |
| **Department**   | D_Num, D_Name, Location, Sup_SSN, HireDate |
| **EMP-Project**  | P-Number, SSN, NumberOfHours |
| **Dependent**    | Name, SSN, D-BirthDate, Gender |

## SQL Queries (DDL)

```sql
-- Create DB
CREATE DATABASE Company;

USE Company;

-- Create Tables

-- Employee table
CREATE TABLE employees (
    ssn INT PRIMARY KEY,
    first_name VARCHAR(255),
    last_name VARCHAR(255),
    gender VARCHAR(50),
    birth_date DATE,
    department_num INT,
    sup_ssn INT,
    FOREIGN KEY (sup_ssn) REFERENCES employees(ssn)
);

-- Department table
CREATE TABLE department (
    department_num INT PRIMARY KEY,
    department_name VARCHAR(255),
    department_location VARCHAR(255),
    sup_ssn INT,
    hire_date DATE,
    FOREIGN KEY (sup_ssn) REFERENCES employees(ssn)
);

-- Add constraint to column department_num in employees
ALTER TABLE employees 
ADD CONSTRAINT fk_department_num FOREIGN KEY (department_num) REFERENCES department(department_num);

-- Project table
CREATE TABLE project (
    project_number INT PRIMARY KEY,
    project_name VARCHAR(255),
    project_city VARCHAR(255),
    project_location VARCHAR(255),
    department_num INT,
    FOREIGN KEY (department_num) REFERENCES department(department_num)
);

-- Dependents table
CREATE TABLE dependents (
    ssn INT,
    name VARCHAR(255),
    dependent_gender VARCHAR(50),
    dependent_birth DATE,
    PRIMARY KEY (ssn, name),
    FOREIGN KEY (ssn) REFERENCES employees(ssn)
);

-- Employees_Project table
CREATE TABLE employees_project (
    ssn INT,
    project_number INT,
    number_of_hours DECIMAL(5, 2),
    PRIMARY KEY (ssn, project_number),
    FOREIGN KEY (ssn) REFERENCES employees(ssn),
    FOREIGN KEY (project_number) REFERENCES project(project_number)
);
```

## SQL Queries (DML)

```sql
-- ADD VALUES TO department (department_num , department_name, department_location, sup_ssn(NULL), hire_date)
INSERT INTO department
VALUES (1 , 'Human Resources' , 'Cairo' , NULL , '2010-04-15'),
(2, 'Information Technology', 'Alexandria', NULL, '2009-08-20'),
(3, 'Finance', 'Giza', NULL, '2012-06-30'),
(4, 'Marketing', 'Cairo', NULL, '2015-02-12'),
(5, 'Logistics', 'Suez', NULL, '2018-10-03');



-- ADD VALUES TO employees (ssn , first_name ,last_name ,gender ,birth_date ,department_num ,sup_ssn)
INSERT INTO employees
VALUES (1001, 'Ahmed', 'Ali', 'Male', '1990-01-15', 1, 1002),
(1002, 'Mona', 'Hassan', 'Female', '1985-03-25', 2, NULL),
(1003, 'Omar', 'Youssef', 'Male', '1992-07-12', 1, 1002),
(1004, 'Sara', 'Ibrahim', 'Female', '1995-09-18', 3, 1005),
(1005, 'Khaled', 'Mahmoud', 'Male', '1980-11-03', 3, NULL),
(1006, 'Layla', 'Ahmed', 'Female', '1988-02-19', 2, 1002),
(1007, 'Hassan', 'Othman', 'Male', '1991-12-20', 1, 1003),
(1008, 'Nora', 'Farid', 'Female', '1993-06-10', 2, 1006),
(1009, 'Tamer', 'Said', 'Male', '1987-08-05', 3, 1005),
(1010, 'Reem', 'Khalil', 'Female', '1994-11-22', 1, 1003);

-- UPDATE(EDIT) sup_ssn IN department where dnum = 1,2,3,4,5
UPDATE department
SET sup_ssn = CASE
WHEN department_num = 1 THEN 1003
WHEN department_num = 2 THEN 1001
WHEN department_num = 3 THEN 1009
WHEN department_num = 4 THEN 1008
WHEN department_num = 5 THEN 1005
END;

-- SELECT dpartment
SELECT * FROM department

-- SELECT employees
SELECT * FROM employees


-- ADD VALUES TO project (project_number, project_name, project_city, project_location, department_num)
INSERT INTO project
VALUES (201, 'AI Development', 'Cairo', 'Smart Village', 2),
(202, 'Website Revamp', 'Alexandria', 'IT Park', 2),
(203, 'Financial Analysis', 'Giza', 'Downtown', 3),
(204, 'Employee Training', 'Cairo', 'Headquarters', 1),
(205, 'Marketing Strategy', 'Suez', 'Branch Office', 4),
(206, 'Supply Chain Upgrade', 'Suez', 'Warehouse', 5);

-- ADD VALUES TO dependents (ssn, name, dependent_gender, dependent_birth)
INSERT INTO dependents
VALUES (1001, 'Ali Ahmed', 'Male', '2015-05-10'),
(1001, 'Lina Ahmed', 'Female', '2018-09-15'),
(1002, 'Hassan Mona', 'Male', '2010-01-05'),
(1003, 'Youssef Omar', 'Male', '2017-03-08'),
(1004, 'Mariam Sara', 'Female', '2019-07-11'),
(1005, 'Mahmoud Khaled', 'Male', '2016-02-20'),
(1006, 'Rania Layla', 'Female', '2013-10-14');

-- ADD VALUES TO employees_project (ssn, project_number, number_of_hours)
INSERT INTO employees_project
VALUES (1001, 201, 120.5),
(1002, 204, 100.0),
(1003, 202, 80.5),
(1004, 203, 95.0),
(1005, 206, 110.0),
(1006, 201, 75.5),
(1007, 205, 60.0);

--  display all employees (their names and SSNs) along with their departments (department name).
SELECT ssn , first_name ,last_name FROM employees

-- Write a query to display all employees born after 1990.
SELECT * 
FROM employees
WHERE birth_date > '1990-01-01'

SELECT *
FROM employees
WHERE birth_date BETWEEN '1990-01-01' AND '1999-01-01'


-- USE LIKE !
SELECT *
FROM employees
WHERE first_name LIKE '%o__'

-- USE ALIAS (RENAME COLUMN)
SELECT MAX(ssn) AS 'MAX SSN'
FROM employees

-- CONCATE WITH ALIAS
SELECT first_name + ' ' + last_name AS NAME
FROM employees
WHERE ssn > 1001

-- ORDER NAME BY BIRTH_DATE
SELECT ssn , first_name + ' ' + last_name AS NAME , gender , birth_date , department_num , sup_ssn
FROM employees
ORDER BY birth_date DESC

-- DISPLAY DEPARTMENT THAT HAVE EMPLOYEE
SELECT DISTINCT department_num
FROM employees


-- DISPLAY EMP NAME AND DEP NAME that he MANAGES
SELECT first_name + ' ' + last_name AS full_name , department_name
FROM employees ,department
WHERE department.sup_ssn = ssn

-- Display the employee name and the department name that he works for
SELECT first_name + ' ' + last_name AS full_name , department_name
FROM employees ,department
WHERE employees.department_num = department.department_num

--Display the employee name and the department name that he works for (EQUAL JOIN OR INNER JOIN)
SELECT first_name + ' ' + last_name AS full_name , department_name
FROM employees E INNER JOIN department AS D
ON E.department_num = D.department_num

--Display the names of employees, the projects they are working on and the number of hours spent on each project
SELECT first_name + ' ' + last_name AS full_name , project_name , number_of_hours
FROM employees_project , employees , project
WHERE employees_project.project_number = project.project_number AND employees_project.ssn = employees.ssn

SELECT first_name + ' ' + last_name AS full_name , project_name , number_of_hours
FROM employees_project
INNER JOIN  project
ON employees_project.project_number = project.project_number
INNER JOIN  employees
ON employees_project.ssn = employees.ssn

SELECT * FROM department
SELECT * FROM project
SELECT * FROM employees
SELECT * FROM employees_project

-- DISPLAY full_name & department_location
SELECT first_name + ' ' + last_name AS full_name , department_location
FROM employees E LEFT OUTER JOIN department D
ON E.department_num = D.department_num

SELECT first_name + ' ' + last_name AS full_name , department_name
FROM employees E RIGHT OUTER JOIN department D
ON E.department_num = D.department_num

SELECT first_name + ' ' + last_name AS full_name , department_name
FROM employees E FULL OUTER JOIN department D
ON E.department_num = D.department_num

-- LENGTH 1001 --> 4
SELECT LEN(ssn)
FROM employees_project

-- CRAETE VIEW
CREATE VIEW vw_employee_project AS
SELECT first_name + ' ' + last_name AS full_name , project_name , number_of_hours
FROM employees_project
INNER JOIN  project
ON employees_project.project_number = project.project_number
INNER JOIN  employees
ON employees_project.ssn = employees.ssn;

-- RETRIVE VIEW
SELECT * FROM vw_employee_project
ORDER BY number_of_hours;

```
