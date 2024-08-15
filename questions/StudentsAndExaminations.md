# Students And Examinations

## Table: Students

| Column Name   | Type    |
| --- | --- |
| student_id    | int     |
| student_name  | varchar |

- student_id is the primary key (column with unique values) for this table.
- Each row of this table contains the ID and the name of one student in the school.
 

## Table: Subjects

| Column Name  | Type    |
| --- | --- |
| subject_name | varchar |

- subject_name is the primary key (column with unique values) for this table.
- Each row of this table contains the name of one subject in the school.
 

## Table: Examinations

| Column Name  | Type    |
| --- | --- |
| student_id   | int     |
| subject_name | varchar |

- There is no primary key (column with unique values) for this table. It may contain duplicates.
- Each student from the Students table takes every course from the Subjects table.
- Each row of this table indicates that a student with ID student_id attended the exam of subject_name.


## Question

Write a solution to find the number of times each student attended each exam.

Return the result table ordered by student_id and subject_name.

The result format is in the following example.

 

## Example:

Input: 
Students table:

| student_id | student_name |
| --- | --- |
| 1          | Alice        |
| 2          | Bob          |
| 13         | John         |
| 6          | Alex         |


Subjects table:

| subject_name |
| --- |
| Math         |
| Physics      |
| Programming  |


Examinations table:

| student_id | subject_name |
| --- | --- |
| 1          | Math         |
| 1          | Physics      |
| 1          | Programming  |
| 2          | Programming  |
| 1          | Physics      |
| 1          | Math         |
| 13         | Math         |
| 13         | Programming  |
| 13         | Physics      |
| 2          | Math         |
| 1          | Math         |


Output: 

| student_id | student_name | subject_name | attended_exams |
| --- | --- | --- | --- |
| 1          | Alice        | Math         | 3              |
| 1          | Alice        | Physics      | 2              |
| 1          | Alice        | Programming  | 1              |
| 2          | Bob          | Math         | 1              |
| 2          | Bob          | Physics      | 0              |
| 2          | Bob          | Programming  | 1              |
| 6          | Alex         | Math         | 0              |
| 6          | Alex         | Physics      | 0              |
| 6          | Alex         | Programming  | 0              |
| 13         | John         | Math         | 1              |
| 13         | John         | Physics      | 1              |
| 13         | John         | Programming  | 1              |


Explanation: 
The result table should contain all students and all subjects.
Alice attended the Math exam 3 times, the Physics exam 2 times, and the Programming exam 1 time.
Bob attended the Math exam 1 time, the Programming exam 1 time, and did not attend the Physics exam.
Alex did not attend any exams.
John attended the Math exam 1 time, the Physics exam 1 time, and the Programming exam 1 time.

## Solution
```sql
SELECT s.student_id, s.student_name, sub.subject_name, COUNT(e.student_id) AS attended_exams
  FROM Students s
  CROSS JOIN Subjects sub
  LEFT JOIN Examinations e
    ON s.student_id = e.student_id AND sub.subject_name = e.subject_name
  GROUP BY s.student_id, s.student_name, sub.subject_name
  ORDER BY s.student_id, sub.subject_name;
```

# Notes

This question was very difficult.

To solve this problem, you can use SQL to aggregate and count the number of times each student attended each exam. The solution involves a few steps:
1. **CROSS JOIN Students with Subjects:** This creates all possible combinations of students and subjects.
2. **LEFT JOIN the resulting table with Examinations:** This ensures that even if a student hasn’t attended an exam, the combination still exists.
3. **COUNT(e.student_id):** The COUNT function is used to count how many times the student attended a specific exam.
4. **GROUP BY student_id, student_name, subject_name:** Grouping by these columns ensures we get the count of attended exams for each student-subject combination.
5. **ORDER BY student_id, subject_name:** Finally, ordering by student_id and subject_name gives us the desired order.

I was only about to reach the following on my own but couldn't figure out how to do the COUNT for attended_exams column.

```sql
SELECT table_join_1.student_id, table_join_1.student_name, table_join_1.subject_name
,  COUNT(DISTINCT Examinations.subject_name, Examinations.student_id) AS attended_exams
    FROM (SELECT *
        FROM Students
        INNER JOIN Subjects) AS table_join_1
    LEFT JOIN Examinations
        ON table_join_1.student_id = Examinations.student_id
    GROUP BY table_join_1.student_id, table_join_1.subject_name
    ORDER BY table_join_1.student_id, table_join_1.subject_name ASC;
```
my output (it's wrong)- 
| student_id | student_name | subject_name | attended_exams |
| ---------- | ------------ | ------------ | -------------- |
| 1          | Alice        | Math         | 3              |
| 1          | Alice        | Physics      | 3              |
| 1          | Alice        | Programming  | 3              |
| 2          | Bob          | Math         | 2              |
| 2          | Bob          | Physics      | 2              |
| 2          | Bob          | Programming  | 2              |
| 6          | Alex         | Math         | 0              |
| 6          | Alex         | Physics      | 0              |
| 6          | Alex         | Programming  | 0              |
| 13         | John         | Math         | 3              |
| 13         | John         | Physics      | 3              |
| 13         | John         | Programming  | 3              |
