# ERD & Mapping

![NTI](https://github.com/Ahmed-Hamdy-Hlil/Images/blob/main/logo-1.png)

NTI – Data Analysis

---

## CASE 2

Prepare an E-R diagram for a hospital:


### Description:

- A General Hospital consists of a number of specialized wards. Each ward is described by ward_id, Name
- The system records the following details about patients: Patient_id, name, Date_Of_Birth
- Each ward may host more patients and each patient is hosted by only one ward.
- Each patient is assigned to one leading consultant but may be examined by other consultants, if required. 
- Each consultant may be assigned zero or more patients and may examine zero or more patients.
- Consultants are described by Consultant_id, Name
-	The system has to record all required data each time the Nurse gives a patient a certain drug with specified dosage at certain date and time.
-	Each ward is under supervision of one nurse and a nurse may supervise only one ward. 
-	Each Nurse must serve in one ward and ward can have many nurses.
-	Data about the nurse is recorded as her name and her number and her address. 
-	A drug has code number, recommended dosage and more than one brand name

---
### ERD

![ERD](https://github.com/Ahmed-Hamdy-Hlil/Images/blob/main/Problem%203_3.png)

---
### Mapping

![Mapping](https://github.com/Ahmed-Hamdy-Hlil/Images/blob/main/Problem%203_4.png)
