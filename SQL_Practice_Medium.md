# Medium

Show unique birth years from patients and order them by ascending.

```sql
select 
	distinct YEAR(birth_date)
from patients
order by 1 asc;
```

Show unique first names from the patients table which only occurs once in the list.

For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.

```sql
select 
	first_name
from patients
group by first_name
having Count(*) = 1;
```

Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.

```sql
select 
	patient_id,
    first_name
from patients
where first_name LIKE 'S%S' 
and len(first_name)>=6;
```

Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'.

Primary diagnosis is stored in the admissions table.

```sql
SELECT 
	pa.patient_id,
    pa.first_name,
    pa.last_name
FROM patients pa 
JOIN
	admissions ad
  	ON 
    pa.patient_id = ad.patient_id
where 
	ad.diagnosis = 'Dementia';
```

Display every patient's first_name.

Order the list by the length of each name and then by alphbetically

```sql
SELECT 
	first_name
FROM
	patients
ORDER BY len(first_name), first_name;
```

Show the total amount of male patients and the total amount of female patients in the patients table.

Display the two results in the same row.

```sql
SELECT 
	count(*)
FROM
	patients
group by gender;
```

```sql
SELECT 
  (SELECT count(*) FROM patients WHERE gender='M') AS male_count, 
  (SELECT count(*) FROM patients WHERE gender='F') AS female_count;
```

```sql
SELECT 
  SUM(Gender = 'M') as male_count, 
  SUM(Gender = 'F') AS female_count
FROM patients
```

```sql
select 
  sum(case when gender = 'M' then 1 end) as male_count,
  sum(case when gender = 'F' then 1 end) as female_count 
from patients;
```

---

Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.

```sql
SELECT 
	first_name,
    last_name,
    allergies
from
	patients
WHERE
	allergies = 'Penicillin'OR allergies = 'Morphine'
order by 3, 1,2;
```

```sql
SELECT
  first_name,
  last_name,
  allergies
FROM patients
WHERE
  allergies IN ('Penicillin', 'Morphine')
ORDER BY
  allergies,
  first_name,
  last_name;
```

---

Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.

```sql
SELECT 
	patient_id,
    diagnosis
from
	admissions
group by
	patient_id, diagnosis
having
	count(diagnosis) > 1;
```

```sql
SELECT
  patient_id,
  diagnosis
FROM admissions
GROUP BY
  patient_id,
  diagnosis
HAVING COUNT(*) > 1;
```

---

Show the city and the total number of patients in the city.

Order from most to least patients and then by city name ascending.

```sql
SELECT 
	city,
    count(*)
from
	patients
group by
	city
order by
	count(*) DESC,
    city;
```

```sql
SELECT
  city,
  COUNT(*) AS num_patients
FROM patients
GROUP BY city
ORDER BY num_patients DESC, city asc;
```

---

Show first name, last name and role of every person that is either patient or doctor.

The roles are either "Patient" or "Doctor"

```sql
SELECT 
	first_name,
    last_name,
    'Patient' AS role
FROM patients
UNION ALL
SELECT
	first_name,
    last_name,
    'Doctor' AS role
from
	doctors
```

---

Show all allergies ordered by popularity. Remove NULL values from query.

```sql
SELECT 
	allergies,
    COUNT(*) AS total_diagnosis
FROM patients
WHERE allergies IS not null
group by allergies
order by 2 desc;
```

---

Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.

```sql
SELECT 
	first_name,
    last_name,
    birth_date
from
	patients
where
	birth_date between '1970-01-01' AND '1979-12-30'
order bY
	birth_date ASC;
```

---

We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order

EX: SMITH,jane

```sql
SELECT 
	CONCAT(upper(last_name),',',LOWER(first_name)) AS new_name_format
FROM
	patients
order by first_name DESC;
```

---

Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.

```sql
SELECT 
	province_id,
    sum(height) AS SumHeights
from
	patients
group by province_id
having SumHeights > 7000;
```

---

Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni’

```sql
SELECT 
	MAX(weight) - MIN(weight)
from
	patients
where 
	last_name = 'Maroni';
```

---

Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.

```sql
SELECT *
FROM 
	admissions
WHERE patient_id = '542'
and admission_date = (
  select 
  	MAX(admission_date)
  	FROM admissions
  	WHERE patient_id = '542')
```

---

Show all columns for patient_id 542's most recent admission_date.

```sql
SELECT 
	patient_id,
	admission_date,
    discharge_date
FROM 
	admissions
WHERE patient_id = '542'
and admission_date = (
  select 
  	MAX(admission_date)
  	FROM admissions
  	WHERE patient_id = '542')
```

```sql
SELECT *
FROM admissions
WHERE patient_id = 542
GROUP BY patient_id
HAVING
  admission_date = MAX(admission_date)
```

```sql
SELECT *
FROM admissions
WHERE patient_id = 542
ORDER BY admission_date DESC
LIMIT 1
```

```sql
SELECT *
FROM admissions
GROUP BY patient_id
HAVING
  patient_id = 542
  AND max(admission_date)
```

---

Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:

1. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.

2. attending_doctor_id contains a 2 and the length of patient_id is 3 characters

```sql
select 
	patient_id,
    attending_doctor_id,
    diagnosis
from
	admissions
WHERE 
	(patient_id % 2 != 0 and attending_doctor_id IN (1, 5, 19))
	or (attending_doctor_id like '%2%' and lEN(patient_id) = 3);
```