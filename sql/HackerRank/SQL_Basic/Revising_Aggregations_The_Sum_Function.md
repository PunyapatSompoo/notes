# Task
Query the total population of all cities in **CITY** where District is **California**.

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
SELECT SUM(population)
FROM city
WHERE district = 'California';
```