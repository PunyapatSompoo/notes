# Task
Query the difference between the maximum and minimum populations in **CITY**.

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
SELECT MAX(population) - MIN(population)
FROM city;
```