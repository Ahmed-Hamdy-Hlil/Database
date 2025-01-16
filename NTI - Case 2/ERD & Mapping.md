# ERD & Mapping

![NTI](logo-1.png)

NTI – Data Analysis

---

## CASE 2

Prepare an E-R diagram for a real estate firm that lists properties for sale. The following describes this organization :


### Description:

- The firm has a number of sales offices in several states. Attributes of sales office include Office_Number and Location.
-	Each sales office is assigned zero or more employees. Attributes of employee include Employee_ID  and Employee_Name. An employee must be assigned to only one sales office.
-	For each sales office, there is always one employee assigned to manage that office and manager can’t manage many sales office at the same time. 
-	The firm lists property for sale. Attributes of property include Property_ID and Location. Components of Location include Address, City, State, and Zip_Code.
-	Each property must be listed with one (and only one) of the sales offices. A sales office may have any number of properties listed, or may have no proper¬ties listed.
-	Each property may have zero or more owners. Attributes of owners are Owner_ID and Owner_Name. An owner own one or more properties. The system stores the percent owned by each owner in each property.
---
### ERD
![ERD](<Problem 2_3.png>)

---
### Mapping
![Mapping](<Problem 2_4.png>)
---

| Entity           | Attributes                       |
|------------------|----------------------------------|
| **EMPLOYEE**          | ID **(PK)**, NAME, OFFICE_Number **(FK)**, Sup_ID **(FK)** |
| **SALES_OFFICE**      | OFFICE_Number **(PK)**, Location, Manger_ID **(FK)** |
| **PROPERTY**   | PROPERTY_ID **(PK)**, Address, City, State, ZIP_Code , OFFICE_Number **(FK)** |
| **OWNER**  | Owner_ID **(PK)**, Owner_Name |
| **OWNER - PROPERTY**    | (Owner_ID **(FK)** , PROPERTY_ID **(FK)** ) **(PK)**, PrecentOwned |

## SQL Queries (DDL)

```sql
CREATE DATABASE firm;

USE firm;

-- CREATE TABLE EMPLOYEE
CREATE TABLE employee(
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(255),
    office_number INT
);

-- CREATE TABLE sales_office
CREATE TABLE sales_office(
    office_number INT PRIMARY KEY,
    location VARCHAR(255),
    manager_id INT,
    FOREIGN KEY(manager_id) REFERENCES employee(employee_id)
);

-- ADD CONSTRAINT
ALTER TABLE employee ADD CONSTRAINT fk_office_number FOREIGN KEY(office_number) REFERENCES sales_office(office_number);

-- CREATE PROPERTY TABLE
CREATE TABLE property(
    property_id INT PRIMARY KEY,
    property_address VARCHAR(255),
    property_city VARCHAR(255),
    property_state VARCHAR(255),
    property_zip_code VARCHAR(255),
    office_number INT,
    FOREIGN KEY(office_number) REFERENCES sales_office(office_number)
);

-- CREATE TABLE OWNERS
CREATE TABLE owners(
    owners_id INT PRIMARY KEY,
    owners_name VARCHAR(255)
);

-- CREATE TABLE OWNER_PROPERTY
CREATE TABLE owner_property(
    owner_id INT,
    property_id INT,
    PRIMARY KEY(owner_id, property_id),
    FOREIGN KEY(owner_id) REFERENCES owners(owners_id),
    FOREIGN KEY(property_id) REFERENCES property(property_id)
);

-- ADD COLUMN SUP_ID TO EMPLOYEE
ALTER TABLE employee ADD sup_id INT;

-- ADD CONSTRAINT TO SUP_ID
ALTER TABLE employee ADD CONSTRAINT fk_sup_id FOREIGN KEY (sup_id) REFERENCES employee(employee_id);

```

