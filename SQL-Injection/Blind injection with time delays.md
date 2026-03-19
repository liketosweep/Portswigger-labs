# SQL Injection: Blind SQLi with Time Delays

**Category:** SQL Injection
**Difficulty:** Practitioner
**Lab Link:** https://portswigger.net/web-security/sql-injection/blind/lab-time-delays

---

## Vulnerability

The application is vulnerable to **blind SQL injection** via a tracking cookie.
There is no visible output or error messages, but the server response time changes based on injected queries.

---

## Payload

```id="x3p9zn"
' || pg_sleep(10)--
```

---

## Exploitation Steps

1. Intercept the request using Burp Suite.

2. Locate the tracking cookie (`TrackingId`).

3. Inject the payload:

```id="q7v2km"
TrackingId=xyz' || pg_sleep(10)--
```

4. Observe that the response is delayed by **10 seconds**.

5. This confirms the SQL injection vulnerability and solves the lab.

---

## Why the Attack Works

* The application executes SQL queries synchronously
* `pg_sleep(10)` pauses execution for 10 seconds
* Delay in response confirms successful injection

This technique is known as **time-based blind SQL injection**.

---

## Prevention

* Use **parameterized queries**
* Avoid inserting user input directly into SQL queries
* Implement strict input validation
