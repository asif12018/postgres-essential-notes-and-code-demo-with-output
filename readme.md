### Handling `NULL` Values with `COALESCE` (PostgreSQL)

**Purpose**
`COALESCE` is used to replace `NULL` values with a default value. It returns the first non-`NULL` value from the given arguments.

---

### Query Example

```sql
SELECT 
  first_name,
  age,
  COALESCE(email, 'Not provided') AS email
FROM students;
```

---

### Sample Data

| first_name | age | email                                 |
| ---------- | --- | ------------------------------------- |
| Asif       | 22  | [asif@mail.com](mailto:asif@mail.com) |
| Rahim      | 21  | NULL                                  |

---

### Output

| first_name | age | email                                 |
| ---------- | --- | ------------------------------------- |
| Asif       | 22  | [asif@mail.com](mailto:asif@mail.com) |
| Rahim      | 21  | Not provided                          |

---

### Reason to Use `COALESCE`

* Prevents `NULL` values in query results
* Improves readability of output data
* Useful for reporting and user-facing responses
* Avoids handling `NULL` in application logic

This approach is commonly used when optional fields (like `email`) may not always contain data.


### Using `LIMIT` and `OFFSET` in PostgreSQL

**Purpose**
`LIMIT` controls how many rows are returned, and `OFFSET` skips a specific number of rows. They are commonly used for pagination.

---

### Query Examples

```sql
-- Page 1
SELECT * 
FROM students 
LIMIT 5 OFFSET 5 * 0;
```

```sql
-- Page 2
SELECT * 
FROM students 
LIMIT 5 OFFSET 5 * 1;
```

---

### How It Works

* `LIMIT 5` → returns 5 rows per page
* `OFFSET 5 * 0` → skips 0 rows (first page)
* `OFFSET 5 * 1` → skips 5 rows (second page)

---

### Sample Output (Conceptual)

**Page 1 (OFFSET 0)**

| student_id | first_name | age |
| ---------- | ---------- | --- |
| 1          | Asif       | 22  |
| 2          | Rahim      | 21  |
| 3          | Karim      | 23  |
| 4          | Nadia      | 20  |
| 5          | Tania      | 22  |

**Page 2 (OFFSET 5)**

| student_id | first_name | age |
| ---------- | ---------- | --- |
| 6          | Rafi       | 24  |
| 7          | Mim        | 21  |
| 8          | Hasan      | 22  |
| 9          | Rima       | 23  |
| 10         | Sakib      | 20  |

---

### Reason to Use `LIMIT` & `OFFSET`



### Using `UPDATE` in PostgreSQL

**Purpose**
The `UPDATE` statement is used to modify existing records in a table based on a condition.

---

### View Data (Before Update)

```sql
SELECT *
FROM students;
```

---

### Update Query

```sql
UPDATE students
SET grade = 'B'
WHERE student_id IN (1, 2);
```

---

### Sample Data (Before)

| student_id | first_name | grade |
| ---------- | ---------- | ----- |
| 1          | Asif       | A     |
| 2          | Rahim      | C     |
| 3          | Karim      | B     |

---

### Output (After Update)

| student_id | first_name | grade |
| ---------- | ---------- | ----- |
| 1          | Asif       | B     |
| 2          | Rahim      | B     |
| 3          | Karim      | B     |

---

### Reason to Use `UPDATE`

* Modify specific rows without affecting the entire table
* Apply changes based on conditions (`WHERE` clause)
* Commonly used for correcting or updating user data

> **Important:** Always use a `WHERE` clause with `UPDATE` to avoid updating all rows unintentionally.


* Implements pagination in APIs and dashboards
* Reduces data load by fetching small chunks
* Improves performance and user experience

This pattern is standard for page-based data retrieval in SQL-backed applications.


### Using `GROUP BY` in PostgreSQL

**Purpose**
`GROUP BY` is used to group rows that share the same values in one or more columns, typically with aggregate functions.

---

### Aggregate Data by Country

```sql
SELECT 
  country,
  AVG(age) AS avg_age,
  MIN(age) AS min_age
FROM students
GROUP BY country;
```

---

### Sample Output

| country    | avg_age | min_age |
| ---------- | ------- | ------- |
| Bangladesh | 21.8    | 20      |
| India      | 22.5    | 21      |
| Nepal      | 23.0    | 22      |

---

### Count Students by Country

```sql
SELECT 
  country,
  COUNT(*) AS total_students
FROM students
GROUP BY country;
```

---

### Sample Output

| country    | total_students |
| ---------- | -------------- |
| Bangladesh | 12             |
| India      | 8              |
| Nepal      | 5              |

---

### Reason to Use `GROUP BY`

* Summarizes large datasets into meaningful statistics
* Works with aggregate functions (`COUNT`, `AVG`, `MIN`, `MAX`, `SUM`)
* Commonly used in reports, dashboards, and analytics queries

`GROUP BY` is essential when you need insights rather than raw row-level data.



### `GROUP BY` with `HAVING` in PostgreSQL

**Purpose**
`HAVING` is used to filter grouped results after aggregation. Unlike `WHERE`, it works with aggregate functions.

---

### Courses with More Than 2 Students

```sql
SELECT 
  course,
  COUNT(*) AS total_students
FROM students
GROUP BY course
HAVING COUNT(*) > 2;
```

---

### Sample Output

| course           | total_students |
| ---------------- | -------------- |
| Computer Science | 6              |
| Mathematics      | 4              |

---

### Countries Where Average Student Age Is Greater Than 21

```sql
SELECT 
  country,
  AVG(age) AS avg_age
FROM students
GROUP BY country
HAVING AVG(age) > 21;
```

---

### Sample Output

| country    | avg_age |
| ---------- | ------- |
| Bangladesh | 21.8    |
| India      | 22.5    |

---

### Reason to Use `HAVING`

* Filters results **after** `GROUP BY` is applied
* Required when conditions involve aggregate functions
* Commonly used for analytical and reporting queries

**Key Difference**

* `WHERE` → filters rows before grouping
* `HAVING` → filters groups after aggregation
* 





### Understanding `INNER JOIN` in PostgreSQL

**Purpose**
`INNER JOIN` returns only the rows that have matching values in both tables based on the join condition.

---

### Query Example

```sql
SELECT 
  p.id AS post_id,
  p.title,
  u.username
FROM posts AS p
INNER JOIN users AS u 
  ON p.user_id = u.id;
```

---

### Example Tables

**posts**

| id | title       | user_id |
| -- | ----------- | ------- |
| 1  | SQL Basics  | 1       |
| 2  | Joins Intro | 2       |
| 3  | Indexing    | 5       |

**users**

| id | username |
| -- | -------- |
| 1  | asif     |
| 2  | rahim    |
| 3  | karim    |

---

### Output

| post_id | title       | username |
| ------- | ----------- | -------- |
| 1       | SQL Basics  | asif     |
| 2       | Joins Intro | rahim    |

---

### Reason to Use `INNER JOIN`

* Retrieves only related data from multiple tables
* Ensures data consistency by excluding unmatched rows
* Commonly used when relationships must exist in both tables

If a post has no matching user, it will not appear in the result set.



### Understanding `LEFT JOIN` in PostgreSQL

**Purpose**
`LEFT JOIN` returns **all rows from the left table** and the matching rows from the right table. If there is no match, the result contains `NULL` values for the right table columns.

---

### Query Example

```sql
SELECT *
FROM posts AS p
LEFT JOIN users AS u
  ON p.user_id = u.id;
```

---

### Example Tables

**posts**

| id | title       | user_id |
| -- | ----------- | ------- |
| 1  | SQL Basics  | 1       |
| 2  | Joins Intro | 2       |
| 3  | Indexing    | 5       |

**users**

| id | username |
| -- | -------- |
| 1  | asif     |
| 2  | rahim    |
| 3  | karim    |

---

### Output

| post_id | title       | user_id | id (user) | username |
| ------- | ----------- | ------- | --------- | -------- |
| 1       | SQL Basics  | 1       | 1         | asif     |
| 2       | Joins Intro | 2       | 2         | rahim    |
| 3       | Indexing    | 5       | NULL      | NULL     |

---

### Reason to Use `LEFT JOIN`

* Keeps all records from the main (left) table
* Identifies missing relationships using `NULL` values
* Useful for reports and audits where incomplete data must be shown

Rows from `posts` are always returned, even if no matching user exists.


### Understanding `RIGHT JOIN` in PostgreSQL

**Purpose**
`RIGHT JOIN` returns **all rows from the right table** and the matching rows from the left table. If there is no match, the result contains `NULL` values for the left table columns.

---

### Query Example

```sql
SELECT *
FROM posts AS p
RIGHT JOIN users AS u
  ON p.user_id = u.id;
```

---

### Example Tables

**posts**

| id | title       | user_id |
| -- | ----------- | ------- |
| 1  | SQL Basics  | 1       |
| 2  | Joins Intro | 2       |
| 3  | Indexing    | 5       |

**users**

| id | username |
| -- | -------- |
| 1  | asif     |
| 2  | rahim    |
| 3  | karim    |

---

### Output

| post_id | title       | user_id | id (user) | username |
| ------- | ----------- | ------- | --------- | -------- |
| 1       | SQL Basics  | 1       | 1         | asif     |
| 2       | Joins Intro | 2       | 2         | rahim    |
| NULL    | NULL        | NULL    | 3         | karim    |

---

### Reason to Use `RIGHT JOIN`

* Keeps all records from the right table
* Highlights users without any related posts
* Helpful when the right table is the primary focus

> **Note:** In practice, `RIGHT JOIN` can often be rewritten as a `LEFT JOIN` by swapping table order for better readability.



### Understanding `FULL JOIN` in PostgreSQL

**Purpose**
`FULL JOIN` (or `FULL OUTER JOIN`) returns **all rows from both tables**. If there is no match, the result contains `NULL` values for the missing side.

---

### Query Example

```sql
SELECT *
FROM posts AS p
FULL JOIN users AS u
  ON p.user_id = u.id;
```

---

### Example Tables

**posts**

| id | title       | user_id |
| -- | ----------- | ------- |
| 1  | SQL Basics  | 1       |
| 2  | Joins Intro | 2       |
| 3  | Indexing    | 5       |

**users**

| id | username |
| -- | -------- |
| 1  | asif     |
| 2  | rahim    |
| 3  | karim    |

---

### Output

| post_id | title       | user_id | id (user) | username |
| ------- | ----------- | ------- | --------- | -------- |
| 1       | SQL Basics  | 1       | 1         | asif     |
| 2       | Joins Intro | 2       | 2         | rahim    |
| 3       | Indexing    | 5       | NULL      | NULL     |
| NULL    | NULL        | NULL    | 3         | karim    |

---

### Reason to Use `FULL JOIN`

* Retrieves complete data from both tables
* Shows matched and unmatched records together
* Useful for data reconciliation and comparison

`FULL JOIN` is ideal when you need a comprehensive view of relationships across tables, including missing links on either side.



