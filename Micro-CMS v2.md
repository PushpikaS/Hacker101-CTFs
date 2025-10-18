# Micro-CMS v2  
- Platform: Hacker101 <br>
- Category: Web <br>
- Difficulty: Medium

## Flag0 - Authentication Bypass via SQL Injection <br><br>
Hints: <br>
- Regular users can only see public pages
- Getting admin access might require a more perfect union
- Knowing the password is cool, but there are other approaches that might be easier <br><br>

The site changelog stated edits require admin access, suggesting an admins table in the backend. <br><br>
<img width="1918" height="333" alt="image" src="https://github.com/user-attachments/assets/8b3fbddf-e367-4922-9811-b4f2e5664f0f" /> <br><br>
<img width="940" height="393" alt="image" src="https://github.com/user-attachments/assets/2d1d4ef0-b629-4dbb-a742-b1b026af910b" /> <br><br>

The payload injects a UNION SELECT that adds a synthetic row to the query results where the password column is the literal 'pass'. The application then compared the submitted password (pass) against the returned rows, found the injected password='pass' row, granting admin access to the protected page /page/3.  <br>
<br> Injected (username field): <br>
```' UNION SELECT 'pass' AS password FROM admins WHERE '1' = '1 ```  <br>
<br> Password supplied: <br>
```pass```  <br>

This resulted in successful authentication and the flag being revealed on the private page. <br><br>
<img width="900" height="247" alt="image" src="https://github.com/user-attachments/assets/9d00fe1d-a474-4280-ab83-37f407b2645d" /> <br>

Remediation: <br>
Parameterize queries, hash passwords securely, authenticate in application logic, enforce least-privilege DB access, and validate endpoints via DAST/SQLi tests.
<br><br>

## Flag1 - Method-based Authentication Bypass <br><br>
Hints: <br>
- What actions could you perform as a regular user on the last level, which you can't now?
- Just because request fails with one method doesn't mean it will fail with a different method
- Different requests often have different required authorization <br><br>

The hints point straight at method-based differences: some endpoints accept a different HTTP method (PUT/POST/PATCH/DELETE/OPTIONS) and that method may skip normal authentication checks or expose extra functionality.  <br><br>
The edit endpoint was probed with OPTIONS, which revealed POST as an allowed method. <br>
```curl -i -s -k -X OPTIONS "https://1c96e359f5392fb795391fc45abb62c2.ctf.hacker101.com/page/edit/1"``` <br><br>
<img width="827" height="175" alt="image" src="https://github.com/user-attachments/assets/d9da3c29-7ad4-46e0-9344-c144bc8fd667" /> <br><br>

POST was then invoked on /page/edit/1 and the POST response returned the flag. <br>
```curl -i -s -k -X POST "https://1c96e359f5392fb795391fc45abb62c2.ctf.hacker101.com/page/edit/1"``` <br><br>
<img width="811" height="193" alt="image" src="https://github.com/user-attachments/assets/a3029aea-2244-4d8a-88ca-09efdbb645ab" /> <br><br>

Remediation: <br>
Ensure all HTTP methods enforce consistent authorization checks, validate requests server‑side, and restrict sensitive endpoints to authenticated roles only.
<br><br>

## Flag2 - Credential Extraction via SQL Injection <br><br>
Hints: <br>
- Credentials are secret, flags are secret. Coincidence? <br><br>

The login POST was intercepted in Burp Proxy and sent to Repeater. The POST was sent and the response contained a message which indicated that a valid admin credential was required to access further secrets. <br><br>
<img width="1243" height="683" alt="image" src="https://github.com/user-attachments/assets/c16b7a7c-57ab-4904-93c9-31e62c804b46" /> <br><br>

A POST request to /login was targeted using sqlmap with the username POST parameter specified for testing. Sqlmap discovered the username parameter was injectable (time-based blind and UNION) and enumerated the back-end MySQL database. <br>
```sqlmap -u https://1c96e359f5392fb795391fc45abb62c2.ctf.hacker101.com/login --data "username=abc&password=xyz" -p username --dbms=mysql --dump``` <br><br>
<img width="1185" height="228" alt="image" src="https://github.com/user-attachments/assets/e6e311cf-e5ed-45e8-9983-e9a41ffbe8c7" /> <br><br>

The admins table was dumped, revealing credentials. <br><br>
<img width="383" height="193" alt="image" src="https://github.com/user-attachments/assets/23f91941-f155-4415-a8f0-9a3d1f54c78e" /> <br><br>

The retrieved credentials were used to login and retrieve the flag. <br><br>
<img width="817" height="96" alt="image" src="https://github.com/user-attachments/assets/a99229c6-a300-474a-85fa-22754448447d" /> <br><br>

Remediation: <br>
Prevent SQLi with parameterized queries, store passwords as hashes and verify in app code, restrict DB privileges, and validate fixes via automated DAST (e.g., sqlmap/OWASP ZAP) and monitoring.
<br><br>

  
