# leetcode-code-resources
## Table of Contents
1. [Combine Two Tables](#combine-two-tables)
2. [Second Highest Salary](#second-highest-salary)
---
## Combine Two Tables
Table `Person`:
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| personId    | int     |
| lastName    | varchar |
| firstName   | varchar |
+-------------+---------+
```
Table `Address`:
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| addressId   | int     |
| personId    | int     |
| city        | varchar |
| state       | varchar |
+-------------+---------+
```
Write an SQL query to report the first name, last name, city, and state of each person in the `Person` table. If the address of a `personId` is not present in the `Address` table, report `null` instead.

**Solution**
```sql
SELECT firstName, lastName, city, state
FROM Person AS p
LEFT JOIN Address AS a
ON p.personId = a.personId;
```

**Example Output**
```
+-----------+----------+---------------+----------+
| firstName | lastName | city          | state    |
+-----------+----------+---------------+----------+
| Allen     | Wang     | Null          | Null     |
| Bob       | Alice    | New York City | New York |
+-----------+----------+---------------+----------+
```

## Second Highest Salary
Table `Employee`:
```
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
```
Write an SQL query to report the second-highest salary from the `Employee` table. If there is no second highest salary, the query should report `null`.

**Solution**
```sql
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (
    SELECT MAX(salary) FROM Employee
)
```
