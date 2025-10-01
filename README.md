# String Patterns, Sorting and Grouping in MySQL using database hr.sql

Using phpMyAdmin with hr.sql database

## Objectives

- Simplify a SELECT statement by using string patterns, ranges, or sets of values
- Sort the result set in either ascending or descending order and identify which column to use for the sorting order
- Eliminate duplicates from a result set and further restrict a result set

## Exercise 1: String Patterns

In this exercise, you will go through some SQL problems on String Patterns.

### 1. Retrieve all employees whose address is in Elgin,IL

```sql
SELECT *
FROM employees
WHERE ADDRESS LIKE '%Elgin,IL%';
```

### 2. Retrieve all employees who were born during the 1970's

```sql
SELECT *
FROM employees
WHERE B_DATE LIKE '197%';
```

### 3. Retrieve all employees in department 5 whose salary is between 60000 and 70000

```sql
SELECT *
FROM employees
WHERE DEP_ID = 5
AND SALARY BETWEEN 60000 AND 70000;
```

## Exercise 2: Sorting

### 1. Retrieve a list of employees ordered by department ID

```sql
SELECT *
FROM employees
ORDER BY DEP_ID;
```

### 2. Retrieve a list of employees ordered in descending order by department ID and within each department ordered alphabetically in descending order by last name

```sql
SELECT *
FROM employees
ORDER BY DEP_ID DESC, L_NAME DESC;
```

### 3. Use department name instead of department ID. Retrieve a list of employees ordered by department name, and within each department ordered alphabetically in descending order by last name

```sql
SELECT E.*
FROM employees E
INNER JOIN departments D ON E.DEP_ID = D.DEPT_ID_DEP
ORDER BY D.DEP_NAME, E.L_NAME DESC;
```

## Exercise 3: Grouping

### 1. For each department ID retrieve the number of employees in the department

```sql
SELECT DEP_ID, COUNT(*) AS NUM_EMPLOYEES
FROM employees
GROUP BY DEP_ID;
```

### 2. For each department retrieve the number of employees in the department, and the average employee salary in the department

```sql
SELECT DEP_ID, COUNT(*) AS NUM_EMPLOYEES, AVG(SALARY) AS AVG_SALARY
FROM employees
GROUP BY DEP_ID;
```

### 3. Label the computed columns in the result set of SQL exercise 2 as NUM_EMPLOYEES and AVG_SALARY

```sql
SELECT DEP_ID, COUNT(*) AS NUM_EMPLOYEES, AVG(SALARY) AS AVG_SALARY
FROM employees
GROUP BY DEP_ID;
```

### 4. Order the result set by Average Salary

```sql
SELECT DEP_ID, COUNT(*) AS NUM_EMPLOYEES, AVG(SALARY) AS AVG_SALARY
FROM employees
GROUP BY DEP_ID
ORDER BY AVG_SALARY;
```

### 5. Limit the result to departments with fewer than 4 employees

```sql
SELECT DEP_ID, COUNT(*) AS NUM_EMPLOYEES, AVG(SALARY) AS AVG_SALARY
FROM employees
GROUP BY DEP_ID
HAVING COUNT(*) < 4
ORDER BY AVG_SALARY;
```

# String Patterns, Sorting and Grouping in MySQL using database PETRESCUE-CREATE.sql

## Exercise 2: Aggregate Functions

### Query A1: Calculate the total cost of all animal rescues

```sql
SELECT SUM(COST) FROM PETRESCUE;
```

### Query A2: Display total cost in a column called SUM_OF_COST

```sql
SELECT SUM(COST) AS SUM_OF_COST FROM PETRESCUE;
```

### Query A3: Display the maximum quantity of animals rescued

```sql
SELECT MAX(QUANTITY) FROM PETRESCUE;
```

### Query A4: Display the average cost of animals rescued

```sql
SELECT AVG(COST) FROM PETRESCUE;
```

### Query A5: Display the average cost of rescuing a dog

```sql
SELECT AVG(COST) FROM PETRESCUE WHERE ANIMAL = 'Dog';
```

## Exercise 3: Scalar and String Functions

### Query B1: Display the rounded cost of each rescue

```sql
SELECT ROUND(COST) FROM PETRESCUE;
```

### Query B2: Display the length of each animal name

```sql
SELECT LENGTH(ANIMAL) FROM PETRESCUE;
```

### Query B3: Display the animal name in uppercase

```sql
SELECT UPPER(ANIMAL) FROM PETRESCUE;
```

### Query B4: Display the animal name in uppercase without duplications

```sql
SELECT DISTINCT UPPER(ANIMAL) FROM PETRESCUE;
```

### Query B5: Display all columns where animals rescued are cats (use lowercase)

```sql
SELECT * FROM PETRESCUE WHERE LOWER(ANIMAL) = 'cat';
```

## Exercise 4: Date and Time Functions

### Query C1: Display the day of the month when cats have been rescued

```sql
SELECT DAY(RESCUEDATE) FROM PETRESCUE WHERE ANIMAL = 'Cat';
```

### Query C2: Display the number of rescues on the 5th month

```sql
SELECT COUNT(*) FROM PETRESCUE WHERE MONTH(RESCUEDATE) = 5;
```

### Query C3: Display the number of rescues on the 14th day of the month

```sql
SELECT COUNT(*) FROM PETRESCUE WHERE DAY(RESCUEDATE) = 14;
```

### Query C4: Display the third day from each rescue (vet appointment)

```sql
SELECT DATE_ADD(RESCUEDATE, INTERVAL 3 DAY) FROM PETRESCUE;
```

### Query C5: Display the length of time animals have been rescued

```sql
SELECT DATEDIFF(CURDATE(), RESCUEDATE) FROM PETRESCUE;
```