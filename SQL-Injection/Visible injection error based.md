SQL Injection: Visible Error-Based SQLi

Category: SQL Injection
Difficulty: Practitioner
Lab Link: https://portswigger.net/web-security/sql-injection/blind/lab-sql-injection-visible-error-based

---

Vulnerability

The application is vulnerable to SQL injection via a tracking cookie.
The query results are not displayed, but database error messages are visible, which can be used to extract data.

---

Payload

' AND (SELECT CAST((SELECT password FROM users WHERE username='administrator') AS int))--

---

Exploitation Steps

1. Intercept the request using Burp Suite.

2. Locate the tracking cookie ("TrackingId").

3. Inject the payload:

TrackingId=xyz' AND (SELECT CAST((SELECT password FROM users WHERE username='administrator') AS int))--

4. The database attempts to cast the password (string) into an integer.

5. This causes an error that includes the administrator password in the response.

6. Use the leaked password to log in as administrator.

---

Why the Attack Works

- The application exposes database error messages
- "CAST(... AS int)" forces a type conversion
- Converting a string to an integer fails, causing an error
- The error message includes the actual query result (password)

This allows direct extraction of sensitive data.

---

Prevention

- Use parameterized queries
- Disable detailed database error messages
- Validate and sanitize user input
