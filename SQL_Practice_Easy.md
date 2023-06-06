# Facil

```sql
SELECT 
	first_name,
    last_name, 
    gender
FROM patients
WHERE
	gender=='M';
```

```sql
SELECT 
	first_name,
    last_name 
from patients
WHERE 
	allergies IS NULL;
```

Show first name of patients that start with the letter 'C’

```sql
SELECT 
	first_name
from patients
WHERE 
	first_name like 'C%';
```

Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)

```sql
SELECT 
	first_name,
    last_name
from patients
WHERE 
	weight <=120 
    AND weight >=100;
```

Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA’

```sql
UPDATE patients
SET allergies = 'NKA'
WHERE allergies IS NULL;
```

Show first name, last name, and the **full** province name of each patient. Example: 'Ontario' instead of 'ON'

```sql
select
	pa.first_name,
    pa.last_name,
    pro.province_name
from 
	patients pa 
JOIN
	province_names pro 
ON
	pa.province_id = pro.province_id;
```

Show how many patients have a birth_date with 2010 as the birth year.

```sql
select
	count(patient_id)
FROM  patients
WHERE birth_date LIKE '2010%';
```

Show the first_name, last_name, and height of the patient with the greatest height.

```sql
select
	first_name,
    last_name,
	max(height)
FROM patients;
```

Show all columns for patients who have one of the following patient_ids: 1,45,534,879,1000

```sql
select *
FROM patients
where patient_id in 
	(1, 45, 534, 879, 1000);
```

Show the total number of admissions

```sql
select COUNT(*)
FROM admissions;
```

Show all the columns from admissions where the patient was admitted and discharged on the same day

```sql
select *
FROM admissions
where 
	admission_date == discharge_date;
```

Show the patient id and the total number of admissions for patient_id 579.

```sql
select 
	patient_id,
    count(admission_date)
FROM admissions
where 
	patient_id == 579
group by patient_id;
```

Based on the cities that our patients live in, show unique cities that are in province_id 'NS'?

```sql
select 
	distinct city
FROM patients
where 
	province_id == 'NS'
```

Write a query to find the first_name, last name and birth date of patients who has height greater than 160 and weight greater than 70

```sql
select 
	first_name,
    last_name,
    birth_date
from patients
where height > 160 AND weight > 70;
```

Write a query to find list of patients first_name, last_name, and allergies from Hamilton where allergies are not null

```sql
select 
	first_name,
    last_name,
    allergies
from patients
where 
	city == 'Hamilton'
    AND
    allergies IS NOT NULL;
```

Based on cities where our patient lives in, write a query to display the list of unique city starting with a vowel (a, e, i, o, u). Show the result order in ascending by city.

```sql
select 
	distinct city
from patients
where 
	city LIKE 'A%'
    or
    city LIKE 'E%'
    or
    city LIKE 'I%'
    or
    city LIKE 'O%'
    or
    city LIKE 'U%'
order by city ASC;
```

```sql
select 
	distinct YEAR(birth_date)
from patients
order by 1 asc;
```