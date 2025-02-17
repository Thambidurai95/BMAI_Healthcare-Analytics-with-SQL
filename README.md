# BMAI_Healthcare-Analytics-with-SQL
![Healthcare Logo](https://github.com/user-attachments/assets/e9923d67-2577-4aea-a639-ab4f49256f8c)

**Creating database and importing table datasets:**

![image](https://github.com/user-attachments/assets/15b1171e-5057-46a5-a681-e8a82b47b98b)

**Assigned Primary key, Foreign key, Unique key and updated the required datatypes for the updated table datasets:**

**ER Diagram to show the relationship between the table datasets:**

![ER Diagram](https://github.com/user-attachments/assets/ada697fe-7e01-4a05-a400-1250a1f1b2a2)

**SQL Tasks:**

**Inner and Equi Joins
Task: Write a query to fetch details of all completed appointments, including the patient’s name, doctor’s name, and specialization.**

select patients.name as patients_name,doctors.name as doctors_name,specialization from patients
inner join appointments on patients.patient_id=appointments.patient_id
inner join doctors on appointments.doctor_id=doctors.doctor_id
where status = 'Completed';

**Left Join with Null Handling
Task: Retrieve all patients who have never had an appointment. Include their name, contact details, and address in the output.**

select name as patients_name,contact_number,address from patients
left join appointments on patients.patient_id=appointments.patient_id
where appointments.patient_id is NULL;

**Right Join and Aggregate Functions
Task: Find the total number of diagnoses for each doctor, including doctors who haven’t diagnosed any patients. Display the doctor’s name, specialization, and total diagnoses.**

select distinct name as doctors_name,specialization,count(diagnosis) as Total_diagnosis from diagnoses
right join doctors on diagnoses.doctor_id=doctors.doctor_id
group by doctors_name,specialization;

**Full Join for Overlapping Data
Task: Write a query to identify mismatches between the appointments and diagnoses tablqes. Include all appointments and diagnoses with their corresponding patient and doctor details.**

# Using Full join, couldn't receive the required output for this task.
# Using Union of Left join and Right join method, received the required output.

select appointment_id,appointments.Patient_id,appointments.doctor_id, null as diagnosis_id, null as diagnosis from appointments
left join patients on appointments.Patient_id=patients.Patient_id
left join doctors on appointments.doctor_id=doctors.doctor_id
left join diagnoses on appointments.Patient_id=diagnoses.Patient_id and appointments.doctor_id=diagnoses.doctor_id
where diagnoses.diagnosis_id is null
union
select null as appointment_id,diagnoses.Patient_id,diagnoses.doctor_id, diagnoses.diagnosis_id,diagnoses.diagnosis from diagnoses
left join patients on diagnoses.Patient_id=patients.Patient_id
left join doctors on diagnoses.doctor_id=doctors.doctor_id
left join appointments on diagnoses.Patient_id=appointments.Patient_id and diagnoses.doctor_id=appointments.doctor_id
where appointments.appointment_id is null;

**Window Functions (Ranking and Aggregation)
Task: For each doctor, rank their patients based on the number of appointments in descending order.**

select doctor_id,patient_id,count(patient_id) as Total_appointments,rank()
over (order by count(patient_id) desc) as Patient_ranks from appointments
group by doctor_id,patient_id;

**Conditional Expressions
Task: Write a query to categorize patients by age group (e.g., 18-30, 31-50, 51+). Count the number of patients in each age group.**

select
	case
		when age >='18' and age <='30' then '18_30'
		when age >='31' and age <='50' then '31_50'
		when age >='51' and age <='70' then '51_70'
		when age >='71' and age <='90' then '71_90'
	else 'no_group'
	end as age_group, count(patient_id) as Total_patients_count
from patients group by age_group;

**Numeric and String Functions
Task: Retrieve a list of patients whose contact numbers end with "1234" and display their names in uppercase.**

select upper(name) as Patients_name from patients where contact_number like '%1234';

**Subqueries for Filtering
Task: Find patients who have only been prescribed "Insulin" in any of their diagnoses.**

select distinct patients.Patient_id from patients where patients.Patient_id in (select distinct diagnoses.Patient_id from diagnoses 
join medications on diagnoses.diagnosis_id=medications.diagnosis_id where medication_name = 'Insulin')
and patients.Patient_id not in 
(select distinct diagnoses.Patient_id from diagnoses join medications on diagnoses.diagnosis_id=medications.diagnosis_id
where medication_name != 'Insulin');

**Date and Time Functions
Task: Calculate the average duration (in days) for which medications are prescribed for each diagnosis.**

select diagnoses.diagnosis_id,diagnosis,medication_name,avg(datediff(end_date,start_date)) as average_duration_days from medications
join diagnoses on medications.diagnosis_id=diagnoses.diagnosis_id
group by diagnoses.diagnosis_id,medication_name;

**Complex Joins and Aggregation
Task: Write a query to identify the doctor who has attended the most unique patients. Include the doctor’s name, specialization, and the count of unique patients.**

select name as doctors_name,specialization,count(distinct patient_id) as unique_patients_count from doctors
inner join appointments on doctors.doctor_id=appointments.doctor_id
group by doctors_name,specialization;








