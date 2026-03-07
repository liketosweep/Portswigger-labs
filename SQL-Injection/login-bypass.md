SQL Injection: Login Bypass

Category: SQL Injection
Difficulty: Apprentice
Lab Link: https://portswigger.net/web-security/sql-injection/lab-login-bypass

---

Vulnerability

The login functionality is vulnerable to SQL injection because the application directly inserts user input into the SQL query without proper sanitization.

This allows an attacker to manipulate the query logic and bypass authentication.

---

Payload

administrator'--

---

Exploitation Steps

1. Navigate to the login page.
2. Enter the following credentials:

Username

administrator'--

Password

anything

3. Submit the login form.

4. The SQL injection comments out the password condition, allowing login as the administrator.

---

Why the Attack Works

The backend query likely resembles:

SELECT * FROM users WHERE username='administrator' AND password='password'

After injection it becomes:

SELECT * FROM users WHERE username='administrator'--' AND password='password'

- "--" comments out the rest of the query.
- The password check is ignored.

As a result, the application authenticates the attacker as administrator.

---

Prevention

- Use parameterized queries / prepared statements
- Validate and sanitize user input
- Implement secure authentication mechanisms
