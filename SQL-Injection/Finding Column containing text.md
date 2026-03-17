SQL Injection: Finding a Column Containing Text (UNION)

Category: SQL Injection
Difficulty: Practitioner
Lab Link: https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text

---

Vulnerability

The application is vulnerable to SQL injection via the "category" parameter.
This allows the use of UNION-based SQL injection to inject additional data into the query results.

---

Payload

' UNION SELECT NULL,'test',NULL--

---

Exploitation Steps

1. Navigate to a product category.

2. Determine the number of columns (from previous lab):

3 columns

3. Identify which column supports text by injecting a string:

' UNION SELECT 'test',NULL,NULL--
' UNION SELECT NULL,'test',NULL--
' UNION SELECT NULL,NULL,'test'--

4. Observe where the string appears in the response.

5. The column where the text is reflected is the injectable text column, solving the lab.

---

Why the Attack Works

For a UNION query to succeed:

- Column count must match
- Data types must match

By inserting a string ("'test'") into different columns, we identify which column supports text data and is reflected in the response.

---

Prevention

- Use parameterized queries
- Validate and sanitize user input
- Avoid direct SQL query construction using user input
