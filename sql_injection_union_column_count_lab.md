Lab Title: SQL Injection UNION Attack – Determining the Number of Columns Returned by the Query

Overview:
This lab demonstrates a common SQL injection vulnerability in the product category filter of a web application. The lab’s objective is to determine the number of columns returned by a SQL query using a UNION-based SQL injection attack. Understanding the column count is critical to successfully crafting a UNION SELECT payload for further exploitation.

Objective:
Determine the number of columns returned by the original SQL query by performing a UNION-based SQL injection that appends NULL values until the injected query executes successfully.

Tools Required:
- Web browser
- Burp Suite (or similar HTTP interceptor/proxy)

Steps to Reproduce:

1. **Intercept the Request:**
   - Use Burp Suite to capture the HTTP request generated when selecting a product category on the website.
   - Example request:  
     `GET /filter?category=Gifts HTTP/1.1`

2. **Initiate the UNION Injection:**
   - Modify the `category` parameter and insert a UNION SELECT with a single `NULL` value:  
     `' UNION SELECT NULL--`
   - Observe the response. If an error occurs, the number of columns in the original query is more than one.

3. **Incrementally Add NULLs:**
   - Modify the injection to include additional `NULL` values, increasing the count each time:  
     `' UNION SELECT NULL, NULL--`  
     `' UNION SELECT NULL, NULL, NULL--`  
     Continue until the response does not return an error and additional content is displayed.

4. **Identify the Correct Column Count:**
   - The correct number of columns is determined when the error disappears and the response contains injected NULL values. For example, if the query succeeds with three NULLs:
     `' UNION SELECT NULL, NULL, NULL--`
   - This indicates the original query returns 3 columns.

Conclusion:
This lab demonstrates the foundational step in UNION-based SQL injection — identifying the number of columns in the result set. It is essential for constructing valid UNION SELECT statements, which can be used to retrieve sensitive information from the database. Best practices such as input validation, prepared statements, and web application firewalls should be implemented to prevent such vulnerabilities.

