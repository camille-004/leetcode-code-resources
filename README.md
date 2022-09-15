# leetcode-code-resources
## Table of Contents
1. [Easy](#easy)
   1. [Combine Two Tables](#combine-two-tables)
   2. [Employees Earning More Than Their Managers](#employees-earning-more-than-their-managers)
   3. [Duplicate Emails](#duplicate-emails)
   4. [Customers Who Never Order](#customers-who-never-order)
   5. [Delete Duplicate Emails](#delete-duplicate-emails)
   6. [First Login Date](#first-login-date)
   7. [Rising Temperature](#rising-temperature)
2. [Medium](#medium)
   1. [Second Highest Salary](#second-highest-salary)
   2. [Department Highest Salary](#department-highest-salary)
---
## Easy
### Combine Two Tables
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
### Employees Earning More Than Their Managers
Write an SQL query to find the employees who earn more than their managers.

**Example**
```
Input: 
Employee table:
+----+-------+--------+-----------+
| id | name  | salary | managerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | Null      |
| 4  | Max   | 90000  | Null      |
+----+-------+--------+-----------+

Output: 
+----------+
| Employee |
+----------+
| Joe      |
+----------+
```
**Explanation**: Joe is the only employee who earns more than his manager.

**Solution**
```sql
SELECT name AS Employee
FROM Employee AS e1
WHERE salary > (
    SELECT salary 
    FROM Employee AS e2 
    WHERE e2.id = e1.managerId
);
```

### Duplicate Emails
Table `Person`:
```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
```
Write an SQL query to report all the duplicate emails.

**Solution**
```sql
SELECT email AS Email
FROM Person
GROUP BY email
HAVING COUNT(email) > 1
```

**Note**: Remember to use `HAVING` for aggregate operations (i.e., after `GROUP`ing).

### Customers Who Never Order
Table `Customers`:
```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
```

Table `Orders`:
```text
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| customerId  | int  |
+-------------+------+
```
Write an SQL query to report all customers who never order anything.

**Note**: Essentially, report all the customers whose `id`s are not in the `customerId` column or `Orders`.

**Solution**
```sql
SELECT c.name AS Customers
FROM Customers AS c
LEFT JOIN Orders AS o ON o.customerId = c.id
WHERE o.customerId IS NULL;
```

### Delete Duplicate Emails
Table `Person`:
```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
```
Write an SQL query to **delete** all the duplicate emails, keeping only one unique email with the smallest `id`. Note that you are supposed to write a `DELETE` statement and not a `SELECT` one.

**Solution**
```sql
DELETE p1 FROM Person p1
INNER JOIN Person p2
WHERE p1.id > p2.id 
AND p1.email = p2.email;
```

**Notes**
- Self-inner joining is a method to keep a unique ID. Reverse the `>` to keep only the highest ID?

### First Login Date
Table `Activity`:
```text
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |
+--------------+---------+
```
Write an SQL query to report the first login date for each player.

**Solution**
```sql
SELECT player_id, MIN(event_date) AS first_login
FROM Activity
GROUP BY player_id;
```

### Rising Temperature

Table `Weather`:
```text
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
```

Write an SQL query to find all dates' `Id` with higher temperatures compared to its previous dates (yesterday).

**Solution**

```sql
SELECT Weather.id AS Id
FROM Weather
JOIN Weather w
ON DATEDIFF(Weather.recordDate, w.recordDate) = 1
WHERE Weather.temperature > w.temperature;
```

**Notes** 
- Use `DATEDIFF` to get the rows where the previous row's date has a difference of 1.
- Join the table with itself when we need to get extra information within the table.

## Medium
### Second Highest Salary
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

### Department Highest Salary
Table `Employee`:
```text
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |
+--------------+---------+
```

Table `Department`:
```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
```
Write an SQL query to find employees who have the highest salary in each of the departments.

**Solution**
```sql
SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary
FROM Department AS d
JOIN Employee AS e
ON e.departmentId = d.id
WHERE e.salary = (
    SELECT MAX(salary) 
    FROM Employee AS e2 
    WHERE e2.departmentId = e.departmentId
);
```
**Notes**
- Joining `Department` with `Employee` is to get the name of the department.
- The subquery is to get the max salary.

**Example Output**
```text
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Jim      | 90000  |
| Sales      | Henry    | 80000  |
| IT         | Max      | 90000  |
+------------+----------+--------+
```
**Explanation**: Max and Jim both have the highest salary in the IT department and Henry has the highest salary in the Sales department.