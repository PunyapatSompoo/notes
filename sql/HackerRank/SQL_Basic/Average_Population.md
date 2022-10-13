# Task
Query the average population for all cities in **CITY**, rounded down to the nearest integer.

### CITY

| Field       | Type         |  
| :---------- | :----------- |
| ID          | NUMBER       |
| NAME        | VARCHAR2(17) |
| COUNTRYCODE | VARCHAR2(3)  |
| DISTRICT    | VARCHAR2(20) |
| POPULATION  | NUMBER       |

# SOLUTION
```
SELECT ROUND(AVG(population), 0)
FROM city;
```