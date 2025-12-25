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

