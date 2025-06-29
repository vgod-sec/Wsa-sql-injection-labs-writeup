Lab: SQL injection vulnerability allowing login bypass

## Description
This lab contains a SQL injection vulnerability in the login function. To solve the lab, perform a SQL injection attack that logs you in as the `administrator` user and delete the user `carlos`.

## Steps to Reproduce

1. Open the lab and intercept the login request using **Burp Suite**.
2. In the POST request to `/login`, locate the `username` parameter.
3. Modify the `username` parameter to the following payload:
   ```
   administrator'--
   ```
4. Leave the password field blank or any random string.
5. Forward the request.
6. You will be logged in as the administrator.
7. Navigate to the **admin panel** and delete the user `carlos` to solve the lab.

## Payload
```sql
administrator'--
```

## Mitigation
- Use parameterized queries or prepared statements to separate SQL logic from user input.
- Apply strict input validation and use an ORM (Object Relational Mapper) where possible.

## Author
vgod (shivam singh)
