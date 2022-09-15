# leetcode-code-resources
## Table of Contents
1. [Easy](#easy)
   1. [Combine Two Tables](#combine-two-tables)
   2. [Employees Earning More Than Their Managers](#employees-earning-more-than-their-managers)
   3. [Duplicate Emails](#duplicate-emails)
2. [Medium](#medium)
   1. [Second Highest Salary](#second-highest-salary)
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
