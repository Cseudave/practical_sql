# Hard

---

Show all of the patients grouped into weight groups.

Show the total amount of patients in each weight group.

Order the list by the weight group decending.

For example, if they weight 100 to 109 they are placed in the 100 weight group, 110-119 = 110 weight group, etc.

—Una forma fácil y directa es usando CASEight group, 110-119 = 110 weight group, etc.

```sql
SELECT COUNT(*) AS patient_in_group, weight_group
FROM (
    SELECT 
        CASE
            WHEN weight BETWEEN 140 AND 149 THEN 140
            WHEN weight BETWEEN 130 AND 139 THEN 130
            WHEN weight BETWEEN 120 AND 129 THEN 120
            WHEN weight BETWEEN 110 AND 119 THEN 110
            WHEN weight BETWEEN 100 AND 109 THEN 100
            WHEN weight BETWEEN 90 AND 99 THEN 90
            WHEN weight BETWEEN 80 AND 89 THEN 80
            WHEN weight BETWEEN 70 AND 79 THEN 70
            WHEN weight BETWEEN 60 AND 69 THEN 60
            WHEN weight BETWEEN 50 AND 59 THEN 50
            WHEN weight BETWEEN 40 AND 49 THEN 40
            WHEN weight BETWEEN 30 AND 39 THEN 30
            WHEN weight BETWEEN 20 AND 29 THEN 20
            WHEN weight BETWEEN 10 AND 19 THEN 10
            ELSE 0 
        END AS weight_group
    FROM patients
) AS data
GROUP BY weight_group
ORDER BY weight_group DESC;
```

Alternativamente se puede crear las categorías aprovechando su periodicidad con la función FLOOR y una división:

```sql
SELECT
    COUNT(*) AS patien_group,
    FLOOR(weight/10)* 10 AS weight_group
FROM patients
GROUP BY weight_group
ORDER BY weight_group DESC;
```

Se utiliza la expresión **`FLOOR(weight/10)*10`** para dividir el peso de cada paciente por 10, redondear hacia abajo al número entero más cercano y luego multiplicar nuevamente por 10. Esto crea grupos de peso basados en intervalos de 10 unidades, y se le asigna el alias "weight_group" a esta columna calculada.

---

Show patient_id, weight, height, isObese from the patients table.

Display isObese as a boolean 0 or 1. 

Obese is defined as weight(kg)/(height(m)) >= 30.

weight is in units kg.

height is in units cm.

```sql
select
	patient_id,
    weight,
    height,
    (CASE
     	WHEN weight/(power(height/100.0, 2)) >= 30 THEN 1
        ELSE 0
        END) AS isObese
 from patients;
```

---

Show patient_id, first_name, last_name, and attending doctor's specialty.

Show only the patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'

Check patients, admissions, and doctors tables for required information.

```sql
SELECT
	p.patient_id,
    p.first_name,
    p.last_name,
    d.specialty
FROM
	patients p
	JOIN admissions a
    	ON a.patient_id = p.patient_id 
    JOIN doctors d
    	ON a.attending_doctor_id = d.doctor_id
WHERE
	a.diagnosis = 'Epilepsy' AND
    d.first_name = 'Lisa'
```

Usando una tabla temporal

```sql
CREATE TEMPORARY TABLE temp_admissions AS
SELECT a.patient_id, a.attending_doctor_id
FROM admissions a
WHERE a.diagnosis = 'Epilepsy';

SELECT p.patient_id, p.first_name, p.last_name, d.specialty
FROM patients p
JOIN temp_admissions ta ON ta.patient_id = p.patient_id
JOIN doctors d ON d.doctor_id = ta.attending_doctor_id
WHERE d.first_name = 'Lisa';
```

---

All patients who have gone through admissions, can see their medical documents on our site. Those patients are given a temporary password after their first admission. Show the patient_id and temp_password.

The password must be the following, in order:

1. patient_id

2. the numerical length of patient's last_name

3. year of patient's birth_date

```sql
SELECT 
    DISTINCT ps.patient_id,
    CAST(ps.patient_id||length(last_name)||YEAR(birth_date) AS int) AS temp_password 
FROM patients ps
JOIN admissions ad ON ps.patient_id=ad.patient_id
```

Alternativamente usanto CTE

```sql
WITH temp_data AS (
    SELECT 
        p.patient_id,
        CONCAT(p.patient_id, LENGTH(p.last_name), 
				YEAR(p.birth_date)) AS temp_password
    FROM patients p
    JOIN admissions a ON p.patient_id = a.patient_id
)
SELECT DISTINCT patient_id, temp_password
FROM temp_data;
```

---

Each admission costs $50 for patients without insurance, and $10 for patients with insurance. All patients with an even patient_id have insurance.

Give each patient a 'Yes' if they have insurance, and a 'No' if they don't have insurance. Add up the admission_total cost for each has_insurance group.

```sql
WITH data AS (
    SELECT *,
        CASE 
            WHEN patient_id % 2 = 0 THEN 'Yes'
            ELSE 'No' 
        END AS has_insurance 
    FROM admissions
)
SELECT 
    has_insurance,
    CASE 
        WHEN has_insurance = 'Yes' THEN COUNT(has_insurance) * 10
        ELSE COUNT(has_insurance) * 50
    END AS cost_after_insurance
FROM data 
GROUP BY has_insurance;
```

Usando subqueries:

```sql
SELECT 
    has_insurance,
    CASE 
        WHEN has_insurance = 'Yes' THEN count_has_insurance * 10
        ELSE count_has_insurance * 50
    END AS cost_after_insurance
FROM (
    SELECT 
        has_insurance,
        COUNT(*) AS count_has_insurance
    FROM (
        SELECT 
            CASE 
                WHEN patient_id % 2 = 0 THEN 'Yes'
                ELSE 'No' 
            END AS has_insurance 
        FROM admissions
    ) AS data
    GROUP BY has_insurance
) AS subquery;
```

---

Show the provinces that has more patients identified as 'M' than 'F'. Must only show full province_name

```sql
SELECT pr.province_name
FROM patients pa
	JOIN province_names pr 
    ON pa.province_id = pr.province_id
GROUP BY 
    pr.province_name
HAVING
    COUNT( CASE WHEN gender = 'M' THEN 1 END) > COUNT(*) * 0.5;
```

---

We are looking for a specific patient. Pull all columns for the patient who matches the following criteria:

- First_name contains an 'r' after the first two letters.
- Identifies their gender as 'F'
- Born in February, May, or December
- Their weight would be between 60kg and 80kg
- Their patient_id is an odd number
- They are from the city 'Kingston'

```sql
SELECT * 
FROM patients
WHERE 
	first_name LIKE '__%r%'
    AND gender = 'F' 
    AND MONTH(birth_date) IN (2, 5, 12) 
    AND (weight BETWEEN 60 AND 80)
    AND patient_id % 2 <> 0
    AND city = 'Kingston';
```

---

Show the percent of patients that have 'M' as their gender. Round the answer to the nearest hundreth number and in percent form.

```sql
SELECT 
	ROUND(COUNT(CASE WHEN gender='M' THEN 1 END)*100.0/COUNT(*),2) || '%' AS percent_of_male_patients 
FROM patients
```

---

For each day display the total amount of admissions on that day. Display the amount changed from the previous date.

```sql
WITH admission_counts_table AS (
  SELECT admission_date, COUNT(patient_id) AS admission_count
  FROM admissions
  GROUP BY admission_date
  ORDER BY admission_date DESC
)
SELECT
  admission_date, 
  admission_count, 
  admission_count - LAG(admission_count) OVER(ORDER BY admission_date) AS admission_count_change 
FROM admission_counts_table
```

Alternativamente:

```sql
WITH RECURSIVE admission_counts_table AS (
  SELECT admission_date, COUNT(patient_id) AS admission_count
  FROM admissions
  GROUP BY admission_date
  ORDER BY admission_date DESC
), admission_counts_change AS (
  SELECT
    admission_date,
    admission_count,
    LAG(admission_count) OVER (ORDER BY admission_date) AS previous_count,
    admission_count - LAG(admission_count) OVER (ORDER BY admission_date) AS admission_count_change
  FROM admission_counts_table
)
SELECT
  admission_date,
  admission_count,
  admission_count_change
FROM admission_counts_change;
```