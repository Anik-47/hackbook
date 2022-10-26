---
description: A Common Web Vulnerability
---

# SQL Injection

SQL injection usually occurs when you ask a user for input, like their username/user-id, and instead of a name/id, the user gives you an SQL statement that you will **unknowingly** run on your database.

### SQL injection attack occurs when:

1. Unintended data enters a program from an untrusted source.
2. The data is used to dynamically construct a SQL query

### The main consequences are:

* <mark style="color:red;">**Confidentiality**</mark>: Since SQL databases generally hold sensitive data, loss of confidentiality is a frequent problem with SQL Injection vulnerabilities.
* <mark style="color:red;">**Authentication**</mark>: If poor SQL commands are used to check user names and passwords, it may be possible to connect to a system as another user with no previous knowledge of the password.
* <mark style="color:red;">**Authorization**</mark>: If authorization information is held in a SQL database, it may be possible to change this information through the successful exploitation of a SQL Injection vulnerability.
* <mark style="color:red;">**Integrity**</mark>: Just as it may be possible to read sensitive information, it is also possible to make changes or even delete this information with a SQL Injection attack.

