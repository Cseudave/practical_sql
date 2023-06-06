# SQL practice

Status: Not started

### Capítulo 1

```sql
SELECT version();
```

El comando SELECT version() se utiliza para obtener la versión del motor de base de datos que estás utilizando

---

### Capítulo 2

```sql
CREATE TABLE teachers (
    id bigserial,
    first_name varchar(25),
    last_name varchar(50),
    school varchar(50),
    hire_date date,
    salary numeric
);
```

El comando CREATE TABLE se utiliza para crear una nueva tabla llamada "teachers" con columnas como id, nombre, apellido, escuela, fecha de contratación y salario

```sql
INSERT INTO teachers (first_name, last_name, school, hire_date, salary)
VALUES ('Janet', 'Smith', 'F.D. Roosevelt HS', '2011-10-30', 36200),
       ('Lee', 'Reynolds', 'F.D. Roosevelt HS', '1993-05-22', 65000),
       ('Samuel', 'Cole', 'Myers Middle School', '2005-08-01', 43500),
       ('Samantha', 'Bush', 'Myers Middle School', '2011-10-30', 36200),
       ('Betty', 'Diaz', 'Myers Middle School', '2005-08-30', 43500),
       ('Kathleen', 'Roush', 'F.D. Roosevelt HS', '2010-10-22', 38500);
```

El comando INSERT INTO se utiliza para insertar nuevos registros en una tabla de una base de datos.

---

### Capítulo 3

```sql
SELECT * FROM teachers;
```

El comando "SELECT * FROM teachers;" se utiliza para recuperar todos los registros de la tabla "teachers", lo que permite ver toda la información almacenada en esa tabla

```sql
SELECT last_name, first_name, salary FROM teachers;
```

```sql
SELECT first_name, last_name, salary
FROM teachers
ORDER BY salary DESC;
```

El comando ORDER BY se utiliza para ordenar los resultados en orden descendente

```sql
SELECT first_name, last_name, salary
FROM teachers
ORDER BY 3 DESC;
```

En lugar de utilizar el nombre de la columna "salary", se utiliza el número de índice "3" para referirse a esa columna en particular.

```sql
SELECT DISTINCT school
FROM teachers
ORDER BY school;
```

Se utiliza el comando SELECT DISTINCT para seleccionar de forma única los valores de la columna "school" de la tabla "teachers".

```sql
SELECT DISTINCT school, salary
FROM teachers
ORDER BY school, salary;
```

se utiliza el comando SELECT DISTINCT para seleccionar de forma única las combinaciones de valores de las columnas "school" y "salary" de la tabla "teachers".

```sql
SELECT last_name, school, hire_date
FROM teachers
WHERE school = 'Myers Middle School';
```

se utiliza una condición en el comando WHERE para seleccionar solamente las filas que cumplen con ser “Myers Middle School”

| Operador | Descripción |
| --- | --- |
| = | Igual |
| <> | Diferente |
| > | Mayor que |
| < | Menor que |
| >= | Mayor o igual que |
| <= | Menor o igual que |
| BETWEEN | En un rango específico (inclusive) |
| LIKE | Coincide con un patrón especificado |
| IN | Coincide con uno de los valores especificados |
| NOT | Negación de una condición |
| AND | Operador lógico "Y" |
| OR | Operador lógico "O" |
| NOT | Operador lógico de negación |
| ILIKE | Coincide con el patrón sin tener en cuenta mayúsculas o minúsculas |

```sql
SELECT first_name
FROM teachers
WHERE first_name LIKE 'sam%';

SELECT first_name
FROM teachers
WHERE first_name ILIKE 'sam%';
```

Se busca el patrón “sam%” como una wildcard.

En SQL, las wildcards (comodines) son caracteres especiales que se utilizan en combinación con los operadores LIKE o ILIKE dentro de una cláusula WHERE para buscar patrones coincidentes en una cadena de texto. Aquí tienes dos tipos comunes de wildcards:

1. El símbolo de porcentaje (%): Se utiliza para representar cero o más caracteres en una cadena. Por ejemplo, si se busca '%man' con LIKE, coincidirá con 'man', 'woman', 'superman', etc. El porcentaje puede colocarse al principio, al final o en ambos extremos de la cadena de búsqueda.
2. El guion bajo (_) : Se utiliza para representar un único carácter en una cadena. Por ejemplo, si se busca 'wom_n' con LIKE, coincidirá con 'woman', 'women', etc., ya que el guion bajo puede ser reemplazado por cualquier carácter individual.

---

### Capítulo 4

```sql
CREATE TABLE char_data_types (
    char_column char(10),
    varchar_column varchar(10),
    text_column text
);
```

Se crea una tabla con columnas con diferentes tipos de datos de la clase strings. Los principales son:

| Tipo de dato | Descripción | Ejemplo |
| --- | --- | --- |
| Numérico | Almacena valores numéricos | INTEGER, DECIMAL |
| Caracteres | Almacena cadenas de texto | CHAR, VARCHAR |
| Fecha y hora | Almacena valores de fecha y hora | DATE, TIMESTAMP |
| Booleano | Almacena valores de verdadero o falso | BOOLEAN |
| Binario | Almacena datos binarios (imágenes, archivos, etc.) | BLOB, VARBINARY |
| Enumeración | Almacena un conjunto de valores permitidos | ENUM |
| Geoespacial | Almacena datos relacionados con ubicación geográfica | GEOMETRY, GEOGRAPHY |
| Array | Almacena una colección de valores de un mismo tipo | INTEGER[], VARCHAR[] |
| JSON | Almacena datos en formato JSON | JSON |
| XML | Almacena datos en formato XML | XML |

```sql
COPY char_data_types TO 'C:\YourDirectory\typetest.txt'
WITH (FORMAT CSV, HEADER, DELIMITER '|');
```

Permite exportar datos desde un archivo. Aunque por los permisos puede producir un error. Alternativamente, se puede cambiar los permisos de la carpeta o se puede usar la terminal. 

```sql
SELECT numeric_column,
       CAST(numeric_column AS integer),
       CAST(numeric_column AS text)
FROM number_data_types;
```

La función CAST se utiliza para convertir un valor de un tipo de dato a otro tipo de dato específico

```sql
SELECT timestamp_column::varchar(10)
FROM date_time_types;
```

El operador "::" se utiliza en PostgreSQL para realizar una conversión explícita de tipos de datos.

---

### Capítulo 5

```sql
CREATE TABLE supervisor_salaries (
    id integer GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    town text,
    county text,
    supervisor text,
    start_date text,
    salary numeric(10,2),
    benefits numeric(10,2)
);
```

"id" es una columna de tipo integer que se generará automáticamente utilizando el atributo IDENTITY. Esta columna se define como clave primaria (PRIMARY KEY) que garantizar su unicidad.

```sql
CREATE TEMPORARY TABLE supervisor_salaries_temp 
    (LIKE supervisor_salaries INCLUDING ALL);
```

Se crea una tabla temporal con el nombre dado, indicando con LIKE que será como la otra tabla. La cláusula "INCLUDING ALL" indica que se deben copiar todas las restricciones.

Una tabla temporal se utiliza para almacenar datos temporales en una sesión de base de datos y se elimina automáticamente al final de la sesión o cuando se cierra la conexión.

```sql
DROP TABLE supervisor_salaries_temp;
```

la sentencia DROP TABLE para eliminar la tabla temporal "supervisor_salaries_temp" que has creado previamente.

```sql
COPY us_counties_pop_est_2019 
    (county_name, internal_point_lat, internal_point_lon)
TO 'C:\YourDirectory\us_counties_latlon_export.txt'
WITH (FORMAT CSV, HEADER, DELIMITER '|');
```

Se exportan los datos de la tabla us_counnties_pop_est_2019 seleccionando algunas columnas. Alternativamente se puede usar una query

```sql
COPY (
    SELECT county_name, state_name
    FROM us_counties_pop_est_2019
    WHERE county_name ILIKE '%mill%'
     )
TO 'C:\YourDirectory\us_counties_mill_export.csv'
WITH (FORMAT CSV, HEADER);
```

---

### Capítulo 6

Las operaciones más comunes en SQL:

| Operación | Descripción |
| --- | --- |
| + | Suma |
| - | Resta |
| * | Multiplicación |
| / | División |
| % | Módulo (resto de la división) |
| ABS() | Valor absoluto |
| CEIL() | Redondeo hacia arriba |
| FLOOR() | Redondeo hacia abajo |
| ROUND() | Redondeo a un número específico de decimales |
| SQRT() | Raíz cuadrada |
| POWER() | Potencia |
| EXP() | Exponencial |
| LOG() | Logaritmo natural |
| SIN() | Seno |
| COS() | Coseno |

```sql
SELECT department,
       spend_2019,
       spend_2022,
       round( (spend_2022 - spend_2019) /
                    spend_2019 * 100, 1) AS pct_change
FROM percent_change;
```

La operación generará una nueva columna operando elementos de cada fila de las columnas seleccionadas

```sql
SELECT
    percentile_cont(.5)
    WITHIN GROUP (ORDER BY numbers),
    percentile_disc(.5)
    WITHIN GROUP (ORDER BY numbers)
FROM percentile_test;
```

Se calculan los percentiles continuos y discretos de la columna "numbers" en la tabla "percentile_test".

```sql
SELECT percentile_cont(ARRAY[.25,.5,.75])
       WITHIN GROUP (ORDER BY pop_est_2019) AS quartiles
FROM us_counties_pop_est_2019;
```

Se especifican varios valores de los percentiles que se desean obtener pasando un array

```sql
SELECT unnest(
            percentile_cont(ARRAY[.25,.5,.75])
            WITHIN GROUP (ORDER BY pop_est_2019)
            ) AS quartiles
FROM us_counties_pop_est_2019;
```

Para que los resultados no salgan en una misma columna se puede usar unnest que descompone un array en una tabla resultante, devolviendo tantas filas como elementos del array

---

### Capítulo 7

```sql
CREATE TABLE departments (
    dept_id integer,
    dept text,
    city text,
    CONSTRAINT dept_key PRIMARY KEY (dept_id),
    CONSTRAINT dept_city_unique UNIQUE (dept, city)
);
```

Las constricciones dan reglas a las columnas para procurar la integridad de los datos.

Las principales son:

| Restricción | Descripción | Ejemplo |
| --- | --- | --- |
| Primary Key | Identifica de forma única una fila en una tabla. No se permite que los valores sean nulos. | PRIMARY KEY (id) |
| Foreign Key | Establece una relación entre dos tablas basada en una clave primaria de otra tabla. | FOREIGN KEY (user_id) REFERENCES users(id) |
| Unique | Garantiza que los valores en una columna (o conjunto de columnas) sean únicos. | UNIQUE (email) |
| Not Null | Impide que una columna acepte valores nulos. | NOT NULL |
| Check | Define una condición que los valores de una columna deben cumplir. | CHECK (age > 0) |
| Default | Establece un valor predeterminado para una columna cuando no se proporciona un valor explícito. | DEFAULT 'N/A' |
| Auto Increment | Incrementa automáticamente un valor numérico para una columna en cada inserción. | id INT AUTO_INCREMENT PRIMARY KEY |
| Check Constraint | Define una condición personalizada para restringir los valores en una columna. | CHECK (salary > 0 AND salary < 100000) |
| Unique Constraint | Similar a la restricción UNIQUE, pero se puede aplicar a múltiples columnas. | UNIQUE (first_name, last_name) |
| Referential Integrity | Mantiene la consistencia de los datos entre tablas relacionadas mediante el uso de claves foráneas. | FOREIGN KEY (customer_id) REFERENCES customers(id) |

```sql
SELECT *
FROM employees JOIN departments
ON employees.dept_id = departments.dept_id
ORDER BY employees.dept_id;
```

Se realiza una consulta combinando las filas de dos tablas considerando los elementos que comparten valor en la columna dept_id

| Tipo de JOIN | Descripción | Ejemplo |
| --- | --- | --- |
| INNER JOIN | Devuelve solo las filas que tienen coincidencias en ambas tablas basadas en la condición de unión especificada. | SELECT * FROM tabla1 INNER JOIN tabla2 ON tabla1.columna = tabla2.columna |
| LEFT JOIN | Devuelve todas las filas de la tabla izquierda y las filas coincidentes de la tabla derecha, o NULL si no hay coincidencias. | SELECT * FROM tabla1 LEFT JOIN tabla2 ON tabla1.columna = tabla2.columna |
| RIGHT JOIN | Devuelve todas las filas de la tabla derecha y las filas coincidentes de la tabla izquierda, o NULL si no hay coincidencias. | SELECT * FROM tabla1 RIGHT JOIN tabla2 ON tabla1.columna = tabla2.columna |
| FULL JOIN | Devuelve todas las filas de ambas tablas, incluyendo las filas coincidentes y las que no tienen coincidencias. | SELECT * FROM tabla1 FULL JOIN tabla2 ON tabla1.columna = tabla2.columna |
| CROSS JOIN | Devuelve el producto cartesiano de las dos tablas, es decir, todas las combinaciones posibles entre las filas de ambas tablas. | SELECT * FROM tabla1 CROSS JOIN tabla2 |
| SELF JOIN | Un tipo de JOIN donde una tabla se une a sí misma, creando una pseudo-relación. | SELECT * FROM tabla t1 INNER JOIN tabla t2 ON t1.columna = t2.columna |

```sql
SELECT d20.id,
       d20.school_2020,
       d35.school_2035
FROM district_2020 AS d20 LEFT JOIN district_2035 AS d35
ON d20.id = d35.id
ORDER BY d20.id;
```

Se vuelve más comodo trabajar con JOIN al usar alias para las tablas

```sql
SELECT d20.id,
       d20.school_2020,
       en.enrollment,
       gr.grades
FROM district_2020 AS d20 JOIN district_2020_enrollment AS en
    ON d20.id = en.id
JOIN district_2020_grades AS gr
    ON d20.id = gr.id
ORDER BY d20.id;
```

En SQL, puedes realizar consultas de más de una tabla a la vez utilizando la cláusula JOIN para combinar las tablas basándose en condiciones de unión específicas.

Al anidar o combinar consultas de múltiples tablas, es importante definir correctamente las condiciones de unión para obtener los resultados deseados.

```sql
SELECT * FROM district_2020
UNION
SELECT * FROM district_2035
ORDER BY id;
```

UNION en SQL es un operador que se utiliza para combinar los resultados de dos o más consultas en una única tabla resultante. El operador UNION permite concatenar verticalmente los conjuntos de resultados de las consultas, eliminando duplicados en el proceso. Por lo que deben tener las mismas columnas.

```sql
SELECT * FROM district_2020
INTERSECT
SELECT * FROM district_2035
ORDER BY id;
```

El operador INTERSECT devuelve solo las filas que son comunes a ambas consultas, eliminando cualquier duplicado. Las consultas involucradas en INTERSECT deben tener la misma cantidad de columnas y los tipos de datos de las columnas deben ser compatibles.

```sql
SELECT * FROM district_2020
EXCEPT
SELECT * FROM district_2035
ORDER BY id;
```

El operador EXCEPT devuelve las filas de la primera consulta que no están presentes en la segunda consulta, eliminando cualquier duplicado. Las consultas involucradas en EXCEPT deben tener la misma cantidad de columnas y los tipos de datos de las columnas deben ser compatibles.

---

### Capítulo 8

```sql
CREATE TABLE natural_key_example (
    license_id text,
    first_name text,
    last_name text,
    CONSTRAINT license_key PRIMARY KEY (license_id)
);
```

una Primary Key (clave primaria) se utiliza para identificar de forma única cada fila en una tabla. Una Primary Key consta de una o más columnas que tienen valores únicos y no nulos para cada fila en la tabla. Se utiliza para garantizar la integridad y consistencia de los datos en la tabla.

Una Primary Key puede ser simple, lo que significa que consta de una sola columna, o puede ser compuesta, lo que implica que consta de múltiples columnas.

En cuanto a la Primary Key natural, se refiere a una clave primaria que se basa en una columna o conjunto de columnas que tienen un significado intrínseco en el contexto de los datos almacenados.

```sql
INSERT INTO surrogate_key_example
OVERRIDING SYSTEM VALUE
VALUES (4, 'Chicken Coop', '2021-09-03 10:33-07');
```

El comando OVERRIDING SYSTEM VALUE se utiliza para indicar que se debe ignorar el valor generado automáticamente para una columna de clave primaria (surrogate key) y, en su lugar, se proporciona un valor explícito.

```sql
ALTER TABLE surrogate_key_example ALTER COLUMN order_number RESTART WITH 5;
```

Cuando se utiliza la cláusula ALTER COLUMN con la opción RESTART WITH, se reinicia la secuencia de valores generados automáticamente para una columna de tipo de dato secuencia o serial.

En este caso, se está reiniciando la secuencia de la columna "order_number" para que el próximo valor generado sea 5.

```sql
CREATE TABLE check_constraint_example (
    user_id bigint GENERATED ALWAYS AS IDENTITY,
    user_role text,
    salary numeric(10,2),
    CONSTRAINT user_id_key PRIMARY KEY (user_id),
    CONSTRAINT check_role_in_list CHECK (user_role IN('Admin', 'Staff')),
    CONSTRAINT check_salary_not_below_zero CHECK (salary >= 0)
);
```

Ejemplo de creación de una tabla que usa la constricción check

```sql
-- Drop
ALTER TABLE not_null_example DROP CONSTRAINT student_id_key;

-- Add
ALTER TABLE not_null_example ADD CONSTRAINT student_id_key PRIMARY KEY (student_id);
```

Eliminación de las constricciones en la columna student_id_key. Entonces se le configura una nueva constricción con ADD

```sql
-- Drop
ALTER TABLE not_null_example ALTER COLUMN first_name DROP NOT NULL;

-- Add
ALTER TABLE not_null_example ALTER COLUMN first_name SET NOT NULL;
```

En este caso se elimina la constricción especifica y se agreaga de nuevo

```sql
CREATE INDEX street_idx ON new_york_addresses (street);
```

Un índice en SQL es una estructura de datos que mejora la eficiencia de las consultas al permitir un acceso más rápido a los datos de una tabla. Al crear un índice en una columna específica, se ordenan los valores de esa columna y se crea una referencia rápida para acceder a ellos.

---

### Capítulo 9

Si se crea una tabla en la que se desea cargar datos, estos deben obedecer a las constricciones creadas, como NOT NULL. De lo contrario producirá un error. 

En PostgreSQL, si ejecutas el código **`COPY`** para cargar datos en una tabla que no existe previamente, se generará un error. La tabla debe estar creada antes de ejecutar la instrucción **`COPY`**.

La alternativa es:

```sql
CREATE TABLE pls_fy2017_libraries AS
    (COPY (
        SELECT *
        FROM CSVREAD('C:\YourDirectory\pls_fy2017_libraries.csv')
    ) TO STDOUT);
```

**`TO STDOUT`** indica que los resultados de la consulta se envían a la salida estándar en lugar de guardarlos en un archivo o enviarlos a otro destino.

En este caso, la salida estándar se utiliza como un medio temporal para transferir los datos desde el archivo CSV hacia la tabla que se está creando.

```sql
SELECT count(*)
FROM pls_fy2018_libraries;
```

Devuelve el conteo de todas las filas de la tabla.

En SQL, las funciones de agregación son utilizadas para realizar cálculos y resúmenes sobre conjuntos de datos. Estas funciones toman múltiples filas de una columna o un conjunto de columnas y devuelven un único resultado agregado

Alternativamente se puede especificar los grupos en los que se desea operar, devolviendo una fila por cada uno.

Las principales son:

| Función | Descripción | Ejemplo |
| --- | --- | --- |
| COUNT | Cuenta el número de filas o valores en un conjunto de datos | SELECT COUNT(*) FROM tabla; |
| SUM | Calcula la suma de los valores en una columna numérica | SELECT SUM(columna) FROM tabla; |
| AVG | Calcula el promedio de los valores en una columna numérica | SELECT AVG(columna) FROM tabla; |
| MIN | Devuelve el valor mínimo de una columna | SELECT MIN(columna) FROM tabla; |
| MAX | Devuelve el valor máximo de una columna | SELECT MAX(columna) FROM tabla; |
| GROUP BY | Agrupa los datos por una o varias columnas y aplica una función de agregación en cada grupo | SELECT columna, SUM(columna) FROM tabla GROUP BY columna; |

```sql
SELECT pls18.stabr,
       sum(pls18.visits) AS visits_2018,
       sum(pls17.visits) AS visits_2017,
       sum(pls16.visits) AS visits_2016,
       round( (sum(pls18.visits::numeric) - sum(pls17.visits)) /
            sum(pls17.visits) * 100, 1 ) AS chg_2018_17,
       round( (sum(pls17.visits::numeric) - sum(pls16.visits)) /
            sum(pls16.visits) * 100, 1 ) AS chg_2017_16
FROM pls_fy2018_libraries pls18
       JOIN pls_fy2017_libraries pls17 ON pls18.fscskey = pls17.fscskey
       JOIN pls_fy2016_libraries pls16 ON pls18.fscskey = pls16.fscskey
WHERE pls18.visits >= 0
       AND pls17.visits >= 0
       AND pls16.visits >= 0
GROUP BY pls18.stabr
HAVING sum(pls18.visits) > 50000000
ORDER BY chg_2018_17 DESC;
```

La cláusula HAVING se utiliza en SQL para filtrar los resultados de una consulta que utiliza la cláusula GROUP BY. A diferencia de la cláusula WHERE, que se utiliza para filtrar filas antes de que se aplique la agrupación, la cláusula HAVING permite filtrar los resultados después de aplicar la agrupación.

---

### Capítulo 10

```sql
SELECT length(zip),
       count(*) AS length_count
FROM meat_poultry_egg_establishments
GROUP BY length(zip)
ORDER BY length(zip) ASC;
```

a función de agregación LENGTH para determinar la longitud de la columna "zip”

```sql
UPDATE meat_poultry_egg_establishments
SET st_copy = st;
```

El comando SET se utiliza para especificar qué columna se va a actualizar y qué valor se va a asignar. En este caso, se está asignando el valor de la columna "st" a la columna "st_copy".

```sql
UPDATE meat_poultry_egg_establishments
SET zip = '00' || zip
WHERE st IN('PR','VI') AND length(zip) = 3;
```

En la cláusula SET, se utiliza la concatenación de cadenas para agregar los caracteres "00" al comienzo del valor de la columna "zip".

```sql
UPDATE meat_poultry_egg_establishments establishments
SET inspection_deadline = '2022-12-01 00:00 EST'
WHERE EXISTS (SELECT state_regions.region
              FROM state_regions
              WHERE establishments.st = state_regions.st 
                    AND state_regions.region = 'New England');
```

La cláusula WHERE utiliza una subconsulta para verificar la existencia de registros en la tabla "state_regions" que cumplan ciertas condiciones. La subconsulta se ejecuta para cada fila de la tabla "meat_poultry_egg_establishments" y busca coincidencias en la columna "st" entre las dos tablas.

```sql
START TRANSACTION;
```

La instrucción START TRANSACTION se utiliza en SQL para iniciar una transacción. Una transacción es un conjunto de operaciones que se deben realizar como una unidad lógica, lo que significa que todas las operaciones deben completarse correctamente o ninguna de ellas se llevará a cabo.

```sql
ROLLBACK;
```

La instrucción ROLLBACK en SQL se utiliza para deshacer transacciones no confirmadas o revertir los cambios realizados en una transacción en curso.

```sql
COMMIT;
```

La instrucción COMMIT se utiliza en SQL para confirmar una transacción en curso. Estos cambios se vuelven permanentes y se reflejarán en la base de datos de manera persistente.

---

### Capítulo 11

```sql
SELECT corr(median_hh_income, pct_bachelors_higher)
    AS bachelors_income_r
FROM acs_2014_2018_stats;
```

`CORR()` permite obtener la correlación entre dos columnas

```sql
round(
      corr(median_hh_income, pct_bachelors_higher)::numeric, 2
      ) AS bachelors_income_r,
```

Obtener la correlación y convertirla a tipo númerico y redondear a dos cifras.

```sql
SELECT var_pop(median_hh_income)
FROM acs_2014_2018_stats;
```

`Var_POP()` obtenie la varianza poblacional de la columna, esto indica qué tan dispersos están los valores alrededor de la media en una población completa. Proporciona una medida de la variabilidad de los datos.

```sql
SELECT stddev_pop(median_hh_income)
FROM acs_2014_2018_stats;
```

La función STDDEV_POP calcula la desviación estándar poblacional de un conjunto de datos. La desviación estándar es una medida de dispersión que indica cuánto se alejan los valores individuales de la media.

$$
STDDEV\_POP = \sqrt{\frac{\Sigma((x - μ)^2)} { N})} 
$$

$$
VAR\_POP = \frac{\Sigma((x - μ)^2)} { N})
$$

Donde:

- x representa cada valor individual del conjunto de datos.
- μ es la media de los valores del conjunto de datos.
- N es el número total de elementos en el conjunto de datos.

```sql
<función>(<expresión>) OVER (
    [PARTITION BY <columnas>]
    [ORDER BY <columnas> [ASC|DESC]]
    [ROWS <frame_clause>]
)
```

Proporciona una forma de especificar cómo se debe realizar el cálculo o el análisis dentro de un contexto de ventana definido.

- **`<función>`**: es la función de ventana que se aplicará a la expresión o columna.
- **`<expresión>`**: es la expresión o columna sobre la cual se realizará el cálculo.
- **`PARTITION BY`**: es una cláusula opcional que especifica cómo se deben agrupar los datos en particiones o grupos.
- **`ORDER BY`**: es una cláusula opcional que define el orden en el que se realizará el cálculo dentro de cada partición.
- **`ASC`** o **`DESC`**: es opcional y se utiliza para especificar el orden ascendente o descendente del **`ORDER BY`**.
- **`ROWS <frame_clause>`**: es una cláusula opcional que permite definir el marco de la ventana, que controla qué filas se incluyen en el cálculo. La **`<frame_clause>`** puede ser una de las siguientes opciones:
    - **`UNBOUNDED PRECEDING`**: indica que el marco incluye todas las filas desde el inicio de la partición hasta la fila actual.
    - **`n PRECEDING`**: indica que el marco incluye las últimas **`n`** filas anteriores a la fila actual.
    - **`CURRENT ROW`**: indica que el marco incluye solo la fila actual.
    - **`n FOLLOWING`**: indica que el marco incluye las próximas **`n`** filas después de la fila actual.
    - **`UNBOUNDED FOLLOWING`**: indica que el marco incluye todas las filas desde la fila actual hasta el final de la partición.

```sql
SELECT
    company,
    widget_output,
    rank() OVER (ORDER BY widget_output DESC),
    dense_rank() OVER (ORDER BY widget_output DESC)
FROM widget_companies
ORDER BY widget_output DESC;
```

La función RANK() se utiliza para asignar un rango a cada fila en función del valor de la columna

La función DENSE_RANK() también asigna un rango a cada fila basado en los valores de la columna, pero a diferencia de RANK(), no salta rangos en caso de empates.

```sql
SELECT
    category,
    store,
    unit_sales,
    rank() OVER (PARTITION BY category ORDER BY unit_sales DESC)
FROM store_sales
ORDER BY category, rank() OVER (PARTITION BY category 
        ORDER BY unit_sales DESC);
```

La cláusula PARTITION BY se utiliza para dividir los datos en particiones o grupos basados en la columna "category". Luego, la función RANK() se aplica dentro de cada partición para asignar un rango a cada fila en función de las unit_sales dentro de esa categoría, en orden descendente.

Puedes tener el primero de cada partición

| category | store | unit_sales | rank() |
| --- | --- | --- | --- |
| Electronics | Store A | 50 | 1 |
| Electronics | Store B | 40 | 2 |
| Electronics | Store C | 35 | 3 |
| Furniture | Store D | 60 | 1 |
| Furniture | Store E | 55 | 2 |
| Furniture | Store F | 45 | 3 |
| Furniture | Store G | 40 | 4 |
| Clothing | Store H | 70 | 1 |
| Clothing | Store I | 65 | 2 |
| Clothing | Store J | 60 | 3 |

La cláusula PARTITION BY en SQL se utiliza en conjunto con las funciones de ventana (window functions) para dividir los datos en particiones o grupos basados en una o más columnas específicas.

Cada partición es tratada de manera independiente y se aplica la función de ventana dentro de cada partición.

```sql
Funciones de ventana
```

Las funciones de ventana (window functions) son funciones avanzadas disponibles en SQL que te permiten realizar cálculos y análisis en un conjunto de filas relacionadas en una consulta.

Las funciones de ventana operan en varias filas a la vez y devuelven un valor agregado o transformado para cada fila individual en el conjunto de resultados

A diferencia de las funciones de agregación tradicionales, las funciones de ventana no reducen el conjunto de resultados a una sola fila, sino que generan un resultado para cada fila basado en su ventana específica.

| Función | Descripción |
| --- | --- |
| COUNT(column) | Cuenta el número de filas en el conjunto de resultados, excluyendo valores nulos. |
| SUM(column) | Calcula la suma de los valores en una columna numérica. |
| AVG(column) | Calcula el promedio de los valores en una columna numérica. |
| MIN(column) | Encuentra el valor mínimo en una columna. |
| MAX(column) | Encuentra el valor máximo en una columna. |
| STDDEV(column) | Calcula la desviación estándar de los valores en una columna. |
| VARIANCE(column) | Calcula la varianza de los valores en una columna. |
| CORR(column1, column2) | Calcula el coeficiente de correlación entre dos columnas. |
| COVAR_POP(column1, column2) | Calcula la covarianza poblacional entre dos columnas. |
| COVAR_SAMP(column1, column2) | Calcula la covarianza de una muestra entre dos columnas. |
| ROW_NUMBER() | Asigna un número único a cada fila en el conjunto de resultados, sin tener en cuenta ningún orden específico. |
| RANK() | Asigna un rango a cada fila en el conjunto de resultados según un orden específico, sin saltar rangos. |
| DENSE_RANK() | Asigna un rango a cada fila en el conjunto de resultados según un orden específico, omitiendo rangos en caso de empate. |
| NTILE(n) | Divide el conjunto de resultados en n grupos o "baldes" aproximadamente iguales, asignando un número de baldes a cada fila. |
| LAG(column, offset) | Devuelve el valor de una columna en una fila anterior dentro de la ventana, según un desplazamiento especificado. |
| LEAD(column, offset) | Devuelve el valor de una columna en una fila siguiente dentro de la ventana, según un desplazamiento especificado. |
| FIRST_VALUE(column) | Devuelve el valor de una columna en la primera fila de la ventana. |
| LAST_VALUE(column) | Devuelve el valor de una columna en la última fila de la ventana. |

---

### Capítulo 12

```sql
date_part('year', '2022-12-01 18:37:12 EST'::timestamptz) AS year
```

Se extrae el año de la fecha después de convertirla a timestamptz

```sql
SELECT make_date(2022, 2, 22);
SELECT make_time(18, 4, 30.3);
SELECT make_timestamptz(2022, 2, 22, 18, 4, 30.3, 'Europe/Lisbon');
```

Crear una fecha

```sql
INSERT INTO current_time_example
            (current_timestamp_col, clock_timestamp_col)
    (SELECT current_timestamp,
            clock_timestamp()
     FROM generate_series(1,1000));
```

realiza una inserción en la tabla **`current_time_example`** con los valores del **`current_timestamp`** y **`clock_timestamp()`** para cada valor generado por la serie del 1 al 1000.

En esencia dice insertar en la tabla las columnas con select las columnas de la tabla dada.

```sql
SHOW timezone;
```

```sql
SET TIME ZONE 'US/Pacific';
```

Configura tu zona horaria

```sql
SELECT '1929-09-30'::date - '1929-09-27'::date;
SELECT '1929-09-30'::date + '5 years'::interval;
```

Operar una fecha con intervalos o fechas

```sql
SELECT segment,
       arrival - departure AS segment_duration,
       sum(arrival - departure) OVER (ORDER BY trip_id) AS cume_duration
FROM train_rides;
```

tiliza la función de ventana **`sum`** para calcular la suma acumulativa de las duraciones de los segmentos de viaje.

La duración acumulativa se calcula sumando las duraciones de los segmentos anteriores en orden ascendente según el **`trip_id`**.

| segment | segment_duration | cume_duration |
| --- | --- | --- |
| A | 2 | 2 |
| B | 3 | 5 |
| C | 4 | 9 |
| D | 5 | 14 |
| E | 2 | 16 |

```sql
Más sobre OVER()
```

La forma en que los conjuntos de filas son determinados por la cláusula **`OVER`** en una función de ventana depende de las especificaciones adicionales que se le proporcionen. Hay tres elementos principales que definen el conjunto de filas:

1. Partición (Partition): La cláusula **`PARTITION BY`** permite dividir el conjunto de datos en grupos más pequeños basados en una o varias columnas. Cada grupo formado por la partición se trata como una unidad separada, y las funciones de ventana se aplican individualmente a cada grupo.
2. Orden (Order): La cláusula **`ORDER BY`** especifica el orden en el que se deben procesar las filas dentro de cada partición. Esto determina el orden en el que se aplican las funciones de ventana y cómo se define el conjunto de filas sobre el cual se realiza el cálculo.
3. Marco (Frame): El marco establece un rango específico de filas sobre las cuales se realiza el cálculo de la función de ventana. Puede ser un número fijo de filas antes y después de la fila actual, o se puede definir en función de los valores de las columnas. El marco se utiliza para controlar qué filas se incluyen en el cálculo y cuáles se excluyen.

La combinación de estos elementos (partición, orden y marco) define el conjunto de filas sobre el cual se realiza el cálculo de una función de ventana. Permite aplicar operaciones y cálculos basados en grupos específicos de filas, en un orden específico y con un rango específico de filas para la inclusión en el cálculo. Esto proporciona una mayor flexibilidad en el análisis de datos y permite realizar cálculos más avanzados y contextuales en SQL.

La razón de la acumulación es la función de agregación.

La función **`sum()`** es una función de agregación que toma todos los elementos hasta la fila vigente y realiza una suma acumulada de esos elementos.

---

### Capítulo 13

```sql
SELECT county_name,
       state_name,
       pop_est_2019
FROM us_counties_pop_est_2019
WHERE pop_est_2019 >= (
    SELECT percentile_cont(.9) WITHIN GROUP (ORDER BY pop_est_2019)
    FROM us_counties_pop_est_2019
    )
ORDER BY pop_est_2019 DESC;
```

Las subconsultas en SQL son consultas anidadas que se utilizan dentro de una consulta principal para realizar cálculos más complejos o para filtrar los resultados de la consulta principal. Una su

Las subconsultas pueden aparecer en diferentes partes de una consulta, como la cláusula SELECT, FROM, WHERE o HAVING.

Algunas características clave de las subconsultas son:

1. Las subconsultas pueden devolver un solo valor, una sola fila o varias filas.
2. El resultado de una subconsulta se puede utilizar en una variedad de formas, como una condición de filtro en la cláusula WHERE, una columna en la cláusula SELECT o una tabla en la cláusula FROM.
3. Las subconsultas pueden ser correlacionadas o no correlacionadas. Las subconsultas correlacionadas están vinculadas a la consulta principal mediante una relación entre las tablas involucradas, lo que permite que la subconsulta haga referencia a valores de la fila actual de la consulta principal.
4. Las subconsultas pueden ser anidadas, lo que significa que una subconsulta puede contener otra subconsulta dentro de ella.
5. Es importante tener en cuenta la eficiencia y el rendimiento al utilizar subconsultas, ya que pueden afectar el tiempo de ejecución de una consulta.

```sql
SELECT census.state_name AS st,
       census.pop_est_2018,
       est.establishment_count,
       round((est.establishment_count/census.pop_est_2018::numeric) * 1000, 1)
           AS estabs_per_thousand
FROM
    (
         SELECT st,
                sum(establishments) AS establishment_count
         FROM cbp_naics_72_establishments
         GROUP BY st
    )
    AS est
JOIN
    (
        SELECT state_name,
               sum(pop_est_2018) AS pop_est_2018
        FROM us_counties_pop_est_2019
        GROUP BY state_name
    )
    AS census
ON est.st = census.state_name
ORDER BY estabs_per_thousand DESC;
```

Se puede unir dos subconsultas mediante JOIN

La idea en esencia es utilizar las subqueries como un valor, columna o tabla para operar para obtener el resultado deseado.

```sql
SELECT first_name, last_name
FROM employees
WHERE EXISTS (
    SELECT id
    FROM retirees
    WHERE id = employees.emp_id);
```

La función EXISTS() en SQL se utiliza para verificar si una subconsulta devuelve algún resultado.

La función EXISTS() se utiliza comúnmente en combinación con una cláusula WHERE en una consulta para filtrar los resultados en función de la existencia de ciertos registros en otra tabla.

Una correlated query, también conocida como subconsulta correlacionada, es una subconsulta en SQL donde la subconsulta interna depende de los resultados de la consulta externa. Esto significa que la subconsulta está relacionada o correlacionada con la consulta principal.

```sql
SELECT county_name,
       state_name,
       pop_est_2018,
       pop_est_2019,
       raw_chg,
       round(pct_chg * 100, 2) AS pct_chg
FROM us_counties_pop_est_2019,
     LATERAL (SELECT pop_est_2019 - pop_est_2018 AS raw_chg) rc,
     LATERAL (SELECT raw_chg / pop_est_2018::numeric AS pct_chg) pc
ORDER BY pct_chg DESC;
```

La cláusula LATERAL en SQL se utiliza para realizar una operación de tipo "join" lateral, lo que significa que la subconsulta lateral puede hacer referencia a las columnas de las tablas anteriores en la consulta.

De manera que se pueden usar los valores calculados con LATERAL utilizando un alias para crear las columnas consultadas, casi como si fueran nuevas variables

```sql
SELECT t.first_name, t.last_name, a.access_time, a.lab_name
FROM teachers t
LEFT JOIN LATERAL (SELECT *
                   FROM teachers_lab_access
                   WHERE teacher_id = t.id
                   ORDER BY access_time DESC
                   LIMIT 2) a
ON true
ORDER BY t.id;
```

utiliza la cláusula LATERAL en una operación de unión izquierda (LEFT JOIN) para obtener los datos consultados en la subquery donde los id coincide de ambas tablas

```sql
WITH large_counties (county_name, state_name, pop_est_2019)
AS (
    SELECT county_name, state_name, pop_est_2019
    FROM us_counties_pop_est_2019
    WHERE pop_est_2019 >= 100000
   )
SELECT state_name, count(*)
FROM large_counties
GROUP BY state_name
ORDER BY count(*) DESC;
```

Common Table Expressions (CTEs) son estructuras temporales y nombradas que se utilizan en SQL para crear consultas más legibles y modularizar la lógica de la consulta. Proporcionan una forma de definir una consulta y utilizarla posteriormente en la misma consulta principal o en consultas secundarias.

```sql
WITH cte_name (column1, column2, ...)
AS (
    SELECT column1, column2, ...
    FROM table_name
    WHERE condition
)
SELECT *
FROM cte_name
WHERE column1 = 'value';
```

Alternativamente pudo escribirse:

```sql
SELECT state_name, count(*)
FROM us_counties_pop_est_2019
WHERE pop_est_2019 >= 100000
GROUP BY state_name
ORDER BY count(*) DESC;
```

Donde la Common Table Expression funciona como el WHERE delimitando las filas que se solicitarán

```sql
-- Install the crosstab() function via the tablefunc module
```

La función **`crosstab()`** en PostgreSQL se utiliza para realizar una operación de pivote o transformación de datos en una consulta. Permite convertir filas en columnas, generando una salida en formato de tabla cruzada o matriz.

```sql
SELECT * FROM crosstab(
    'SELECT category_column, row_name_column, value_column FROM source_table ORDER BY category_column, row_name_column',
    'SELECT DISTINCT category_column FROM source_table ORDER BY 1'
) AS ct (row_name_column datatype, category1 datatype, category2 datatype, ...)
```

Sintaxis más general.

```sql
SELECT *
FROM crosstab('SELECT office,
                      flavor,
                      count(*)
               FROM ice_cream_survey
               GROUP BY office, flavor
               ORDER BY office',

              'SELECT flavor
               FROM ice_cream_survey
               GROUP BY flavor
               ORDER BY flavor')

AS (office text,
    chocolate bigint,
    strawberry bigint,
    vanilla bigint);
```

| office | flavor | count |
| --- | --- | --- |
| A | Chocolate | 10 |
| A | Strawberry | 5 |
| A | Vanilla | 8 |
| B | Chocolate | 15 |
| B | Strawberry | 3 |
| B | Vanilla | 12 |
| C | Chocolate | 7 |
| C | Strawberry | 9 |
| C | Vanilla | 6 |

| office | chocolate | strawberry | vanilla |
| --- | --- | --- | --- |
| A | 10 | 5 | 8 |
| B | 15 | 3 | 12 |
| C | 7 | 9 | 6 |

```sql
SELECT office, flavor, count
FROM (
    SELECT office,
           unnest(ARRAY['chocolate', 'strawberry', 'vanilla']) AS flavor,
           unnest(ARRAY[chocolate, strawberry, vanilla]) AS count
    FROM crosstab_data
) AS subquery
ORDER BY office, flavor;
```

Para despivotear la tabla

```sql
SELECT max_temp,
       CASE WHEN max_temp >= 90 THEN 'Hot'
            WHEN max_temp >= 70 AND max_temp < 90 THEN 'Warm'
            WHEN max_temp >= 50 AND max_temp < 70 THEN 'Pleasant'
            WHEN max_temp >= 33 AND max_temp < 50 THEN 'Cold'
            WHEN max_temp >= 20 AND max_temp < 33 THEN 'Frigid'
            WHEN max_temp < 20 THEN 'Inhumane'
            ELSE 'No reading'
        END AS temperature_group
FROM temperature_readings
ORDER BY station_name, observation_date;
```

La cláusula CASE en SQL permite realizar evaluaciones condicionales y realizar diferentes acciones o devolver diferentes valores en función de las condiciones especificadas

---

### Capítulo 14

```sql
SELECT upper('Neal7');
SELECT lower('Randy');
SELECT initcap('at the end of the day');
SELECT initcap('Practical SQL');
```

Las funciones de cadena (string functions) en SQL son operaciones que se aplican a valores de tipo cadena de caracteres. Estas funciones permiten manipular y transformar cadenas de texto de diversas formas.

| Función | Descripción |
| --- | --- |
| upper(string) | Convierte todos los caracteres de la cadena string en mayúsculas. |
| lower(string) | Convierte todos los caracteres de la cadena string en minúsculas. |
| initcap(string) | Convierte la primera letra de cada palabra en la cadena string en mayúscula y las demás letras en minúscula. |
| char_length(string) | Devuelve la cantidad de caracteres en la cadena string, incluyendo espacios en blanco. |
| length(string) | Devuelve la cantidad de caracteres en la cadena string, excluyendo los espacios en blanco al principio y al final. |
| position(substring in string) | Devuelve la posición de la primera aparición de la subcadena substring dentro de la cadena string. |
| trim(string) | Elimina los espacios en blanco al principio y al final de la cadena string. |
| ltrim(string, characters) | Elimina los caracteres especificados en characters al principio de la cadena string. |
| rtrim(string, characters) | Elimina los caracteres especificados en characters al final de la cadena string. |
| left(string, length) | Devuelve los length primeros caracteres de la cadena string. |
| right(string, length) | Devuelve los length últimos caracteres de la cadena string. |
| replace(string, old, new) | Reemplaza todas las apariciones de la subcadena old en la cadena string con la subcadena new. |

```sql
SELECT substring('The game starts at 7 p.m. on May 2, 2024.' from '.+');
```

La función **`substring`** en SQL se utiliza para extraer una subcadena de una cadena de texto más grande.

```sql
SELECT county_name
FROM us_counties_pop_est_2019
WHERE county_name ~* '(lade|lare)'
ORDER BY county_name;
```

El operador **`~*`** es un operador de coincidencia de expresiones regulares en SQL, donde **`~`** indica coincidencia de expresiones regulares y **`*`** indica que la búsqueda debe ser insensible a mayúsculas y minúsculas.

Mientras `!~` significa "no coincide con”

```sql
SELECT regexp_replace('05/12/2024', '\d{4}', '2023');

SELECT regexp_split_to_table('Four,score,and,seven,years,ago', ',');

SELECT regexp_split_to_array('Phil Mike Tony Steve', ' ');

SELECT array_length(regexp_split_to_array('Phil Mike Tony Steve', ' '), 1);
```

Las expresiones regulares (regular expressions o regex) son patrones de búsqueda utilizados para encontrar y manipular texto en bases de datos y otros sistemas.

| Función | Descripción |
| --- | --- |
| regexp_replace | Reemplaza todas las ocurrencias de un patrón en una cadena con un valor especificado. |
| regexp_split_to_table | Divide una cadena en varias filas basadas en un patrón y devuelve una tabla con los resultados. |
| regexp_split_to_array | Divide una cadena en un arreglo basado en un patrón y devuelve un arreglo con los resultados. |
| array_length | Devuelve la longitud de un arreglo en la dimensión especificada. |
| regexp_match | Determina si una cadena coincide con un patrón de expresión regular y devuelve la expresión |
| regexp_matches | Devuelve todas las coincidencias de un patrón de expresión regular en una cadena en forma de un arreglo de cadenas. |
| regexp_instr | Devuelve la posición de la primera coincidencia de un patrón de expresión regular en una cadena |
| regexp_substr | Devuelve una subcadena que coincide con un patrón de expresión regular dentro de una cadena |
| regexp_replace_all | Reemplaza todas las ocurrencias de un patrón en una cadena con un valor especificado. |
| regexp_replace_substring | Reemplaza una subcadena que coincide con un patrón de expresión regular con un valor especificado dentro de una cadena |
| regexp_count | Cuenta el número de ocurrencias de un patrón de expresión regular en una cadena |

```sql
SELECT
    regexp_match(original_text, '(?:C0|SO)[0-9]+') AS case_number,
    regexp_match(original_text, '\d{1,2}\/\d{1,2}\/\d{2}') AS date_1,
    regexp_match(original_text, '\n(?:\w+ \w+|\w+)\n(.*):') AS crime_type,
    regexp_match(original_text, '(?:Sq.|Plz.|Dr.|Ter.|Rd.)\n(\w+ \w+|\w+)\n')
        AS city
FROM crime_reports
ORDER BY crime_id;
```

Existen diversos patrones de expresiones regulares. Los anteriores combinan más de uno para encontrar ciudades o nombres según la estructura. Los principales patrones son:

| Patrón | Descripción | Ejemplo de Coincidencia |
| --- | --- | --- |
| ^abc | Coincide con cadenas que comienzan con "abc". | "abc123" |
| abc$ | Coincide con cadenas que terminan con "abc". | "123abc" |
| a.b | Coincide con cualquier carácter (excepto salto de línea) entre "a" y "b". | "axb", "a1b", "a*b" |
| [abc] | Coincide con cualquiera de los caracteres especificados dentro de los corchetes. | "a", "b", "c" |
| [^abc] | Coincide con cualquier carácter que no esté especificado dentro de los corchetes. | "x", "y", "z" |
| [a-z] | Coincide con cualquier carácter en el rango de "a" a "z". | "a", "b", "c", ..., "z" |
| \d | Coincide con cualquier dígito decimal (0-9). | "0", "1", "2", ..., "9" |
| \D | Coincide con cualquier carácter que no sea un dígito decimal. | "a", "b", "x", "y", "z" |
| \w | Coincide con cualquier carácter alfanumérico (letras y dígitos) y el guión bajo (_). | "a", "B", "7", "_" |
| \W | Coincide con cualquier carácter que no sea alfanumérico ni guión bajo. | "#", "$", "%", "*", "-" |
| \s | Coincide con cualquier espacio en blanco (espacio, tabulación, salto de línea). | " ", "\t", "\n" |
| \S | Coincide con cualquier carácter que no sea un espacio en blanco. | "a", "b", "x", "y", "z", "0", "1", "2", ..., "9", "_", "#", "$", "%" |
| (abc|def) | Coincide con "abc" o "def". | "abc", "def" |
| abc{3} | Coincide con "abccc" (exactamente 3 repeticiones del carácter "c"). | "abccc" |
| abc{2,4} | Coincide con "abcc", "abccc" o "abcccc" (de 2 a 4 repeticiones del carácter "c"). | "abcc", "abccc", "abcccc" |
| abc* | Coincide con "ab", "abc", "abcc", "abccc", etc. (0 o más repeticiones del carácter "c"). | "ab", "abc", "abcc", "abccc", ... |
| abc+ | Coincide con "abc", "abcc", "abccc", |  |
| abc? | Coincide con "ab" o "abc" (0 o 1 repetición del carácter "c"). | "ab", "abc" |
| (abc) | Crea un grupo de captura que coincide con "abc". | "abc" |
| (?:abc) | Crea un grupo sin captura que coincide con "abc". | "abc" |
| (?i)abc | Realiza una coincidencia sin distinción entre mayúsculas y minúsculas, es decir, coincide con "abc", "Abc", "aBc", etc. | "abc", "Abc", "aBc" |
| (?i:abc) | Realiza una coincidencia sin distinción entre mayúsculas y minúsculas, pero sin afectar el grupo de captura, es decir, "(?i:abc)". | "abc", "Abc", "aBc" |
|  |  |  |

```sql
SELECT
    crime_id,
    (regexp_match(original_text, '(?:C0|SO)[0-9]+'))[1]
        AS case_number
FROM crime_reports
ORDER BY crime_id;
```

La expresión **`(regexp_match(...))[1]`** extrae el primer elemento del array devuelto por **`regexp_match`**, que en este caso representa la primera coincidencia encontrada del número de caso.

En SQL, los arrays se manejan utilizando corchetes **`[]`** para acceder a los elementos individuales de un array. Los corchetes se utilizan después del nombre del array y se especifica el índice del elemento que se desea acceder.

```sql
SELECT president, speech_date
FROM president_speeches
WHERE search_speech_text @@ to_tsquery('english', 'Vietnam')
ORDER BY speech_date;
```

En la consulta que has proporcionado, el símbolo **`@@`** se utiliza como el operador de coincidencia de texto completo en PostgreSQL. Específicamente, se utiliza para realizar una búsqueda de texto completo en una columna que tiene un índice de texto completo configurado.

```sql
SELECT president,
       speech_date,
       ts_headline(speech_text, to_tsquery('english', 'tax'),
                   'StartSel = <,
                    StopSel = >,
                    MinWords=5,
                    MaxWords=7,
                    MaxFragments=1')
FROM president_speeches
WHERE search_speech_text @@ to_tsquery('english', 'tax')
ORDER BY speech_date;
```

**`ts_headline(speech_text, to_tsquery('english', 'tax'), ...)`**: La función **`ts_headline`** se utiliza para generar un fragmento del texto del discurso (**`speech_text`**) con las palabras clave especificadas (**`'tax'`**) resaltadas. Los parámetros adicionales dentro del tercer argumento proporcionan opciones para el resaltado, como establecer los delimitadores de inicio y final (**`StartSel`** y **`StopSel`**), el número mínimo y máximo de palabras en un fragmento (**`MinWords`** y **`MaxWords`**), y el número máximo de fragmentos devueltos (**`MaxFragments`**).