# BMAI_Healthcare-Analytics-with-SQL
![Healthcare Logo](https://github.com/user-attachments/assets/e9923d67-2577-4aea-a639-ab4f49256f8c)

**SQL Tasks:**

**Inner and Equi Joins
Task: Write a query to fetch details of all completed appointments, including the patient’s name, doctor’s name, and specialization.**

select patients.name as patients_name,doctors.name as doctors_name,specialization from patients
inner join appointments on patients.patient_id=appointments.patient_id
inner join doctors on appointments.doctor_id=doctors.doctor_id
where status = 'Completed';
