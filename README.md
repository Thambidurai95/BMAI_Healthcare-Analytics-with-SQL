# BMAI_Healthcare-Analytics-with-SQL
![Healthcare Logo](https://github.com/user-attachments/assets/e9923d67-2577-4aea-a639-ab4f49256f8c)

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













