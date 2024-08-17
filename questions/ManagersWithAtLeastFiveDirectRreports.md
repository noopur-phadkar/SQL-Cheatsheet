# Managers With At Least 5 Direct Reports

## Table: Employee

| Column Name | Type    |
| --- | --- |
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |

- id is the primary key (column with unique values) for this table.
- Each row of this table indicates the name of an employee, their department, and the id of their manager.
- If managerId is null, then the employee does not have a manager.
- No employee will be the manager of themself.
 
## Question

Write a solution to find managers with at least five direct reports.

Return the result table in any order.

## Example:

Input: 
Employee table:

| id  | name  | department | managerId |
| --- | --- | --- | --- |
| 101 | John  | A          | null      |
| 102 | Dan   | A          | 101       |
| 103 | James | A          | 101       |
| 104 | Amy   | A          | 101       |
| 105 | Anne  | A          | 101       |
| 106 | Ron   | B          | 101       |

Output: 

| name |
| --- |
| John |

## Solution

```sql
SELECT name 
    FROM Employee AS E1
    WHERE(
        SELECT count(*)
            FROM Employee AS E2 
            GROUP BY E2.managerId 
            HAVING E2.managerId=E1.id
    ) >= 5;
```
