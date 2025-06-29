# Lab: SQL Injection UNION Attack — Retrieving Data from Other Tables

## Lab Description

This lab demonstrates a SQL injection vulnerability in the product category filter of a web application. The application reflects data retrieved from the database in the HTTP response. The objective is to exploit the vulnerability using a **UNION-based SQL injection** to retrieve sensitive data from a separate `users` table.

> **Goal**: Extract all usernames and passwords from the `users` table and use them to authenticate as the `administrator` user.

## Summary of Steps

1. **Intercept and Inspect Requests**
   - Use **Burp Suite** to capture and analyze the HTTP request containing the `category` parameter.
   - Identify where SQL injection is possible.

2. **Determine the Number of Columns**
   - Inject payloads like the following to determine the number of columns:
     ```sql
     ' UNION SELECT NULL,NULL-- 
     ```
   - Incrementally increase the number of `NULL`s until no error is returned.
   - Once successful, determine which columns support **textual output**:
     ```sql
     ' UNION SELECT 'abc','def'-- 
     ```

3. **Extract Data from Another Table**
   - Use the number of columns and inject a payload to extract `username` and `password` from the `users` table:
     ```sql
     ' UNION SELECT username, password FROM users-- 
     ```

4. **Retrieve Administrator Credentials**
   - Note down the output from the previous step.
   - Identify the `administrator` credentials.

5. **Login and Solve the Lab**
   - Use the extracted administrator credentials to log in via the application’s login page.
   - If successful, the lab will be marked as **Solved**.

## Exploit Example

Below is a full payload assuming the vulnerable parameter is `category` and there are two columns:

```http
GET /?category=' UNION SELECT username, password FROM users--
