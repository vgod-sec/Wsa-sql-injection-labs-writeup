# Lab: SQL Injection UNION Attack - Finding a Column Containing Text

##  Objective
This lab contains a SQL injection vulnerability in the product category filter. Your goal is to exploit this vulnerability using a UNION-based SQL injection and identify which column is compatible with string data. Once identified, inject a provided string into that column and have it appear in the response to solve the lab.

##  Tools Required
- Web Browser
- Burp Suite or any HTTP request interceptor
- Basic understanding of SQL Injection and UNION queries


##  Step-by-Step Guide

### 1️⃣ Intercept the Request
Intercept or observe the request that filters products by category. It will look something like this:
```
GET /filter?category=Accessories HTTP/1.1
```
This `category` parameter is vulnerable to SQL injection.


### 2️⃣ Find the Number of Columns
To use a UNION-based injection, you must match the number of columns in the original query. Test this by injecting increasing numbers of `NULL` values:

Try the following payloads in the category parameter:
```
' UNION SELECT NULL-- 
' UNION SELECT NULL, NULL-- 
' UNION SELECT NULL, NULL, NULL-- 
' UNION SELECT NULL, NULL, NULL, NULL-- 
```
Continue increasing `NULL`s until you receive a valid response (no error). This confirms the correct number of columns. For example, if `' UNION SELECT NULL, NULL, NULL--` works, then the query has **3 columns**.


### 3️⃣ Find a Text-Compatible Column
Now, test which of the columns can accept and display text by replacing each `NULL` with a string value like `'test123'`:

Try:
```
' UNION SELECT 'test123', NULL, NULL-- 
' UNION SELECT NULL, 'test123', NULL-- 
' UNION SELECT NULL, NULL, 'test123'-- 
```
Look for the one that causes `'test123'` to appear in the response. The corresponding column is the one compatible with string data.


### 4️⃣ Inject the Lab-Provided String
Now that you know which column accepts text, replace `'test123'` with the **actual string** provided by the lab (e.g., `Qx9Utz`):

Example:
```
' UNION SELECT NULL, 'Qx9Utz', NULL-- 
```
Once this string appears in the application's response, the lab is solved.


## ✅ Summary
- Use UNION SELECT with incremented `NULL`s to determine the number of columns.
- Inject a known string in each column position to find which accepts text.
- Replace the known string with the lab's required string to solve the challenge.
- Ensure payloads are properly URL-encoded if necessary.


## 💡 Tips
- Make use of Burp Suite's repeater to test payloads easily.
- Use comments (`--`) to terminate the original query cleanly.
- If the app filters quotes or spaces, try encoding or alternate syntax.
