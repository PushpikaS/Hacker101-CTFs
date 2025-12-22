# XSS Playground by zseano 
- Platform: Hacker101
- Category: Web
- Difficulty: Moderate

## Flag0
Hints:
- Can you find all of the XSS on this page? Use a keen eye and see if you can discover the following types of XSS:
   - ```5``` Reflective Cross Site Scripting
   - ```3``` Stored Cross Site Scripting
   - ```2``` DOM-Based Cross Site Scripting
   - ```1``` CSP-Bypass Cross Site Scripting
   - ```1``` use of XSS to leak "something"

After loading the challenge page, no flag was visible in the rendered HTML. 
There were no obvious reflections or input‑based behaviors that immediately exposed sensitive data. This suggested that the flag was likely retrieved dynamically rather than embedded directly in the page.
Based on this, the focus shifted to analyzing client‑side behavior.

Reviewing the page source revealed an external JavaScript file: <br>
```<script src="custom.js"></script>``` <br>
<img width="895" height="802" alt="image" src="https://github.com/user-attachments/assets/a14615e0-3d68-42e5-bd41-08059450b4d1" /> <br>
Since JavaScript often contains logic for backend communication, this file was inspected to understand how the application interacts with its API. <br>

Inside custom.js, several functions were found making XMLHttpRequest calls to endpoints under ```/api/action.php```
<br><br>
<img width="1918" height="522" alt="image" src="https://github.com/user-attachments/assets/1c27c020-76ab-4baa-841d-f63aa34771d1" /> <br><br>
One function in particular stood out: 
```
function retrieveEmail(e){
    var t = new XMLHttpRequest;
    t.open("GET","api/action.php?act=getemail",!0),
    t.setRequestHeader("X-SAFEPROTECTION","enNlYW5vb2Zjb3Vyc2U="),
    t.send()
}
```
This revealed:
- A hidden API endpoint: ```act=getemail```
- A custom HTTP header used for access control
- A static header value embedded directly in frontend code

The endpoint name ```getemail``` implied access to sensitive information. Additionally, the request was not exposed through any visible UI interaction and relied solely on a hardcoded header for protection.
Because the header value was visible client‑side, it was clear that the endpoint could be accessed by recreating the request manually. At this stage, the flag source had been identified.

Accessing the endpoint directly through the browser failed, which was expected since browsers do not allow arbitrary custom headers to be set.
This confirmed that a manual request would be required.

Standard curl requests failed, even when the correct header value was supplied. This suggested that the issue was related to how the request was being sent rather than the endpoint itself.

Further testing revealed that the backend compared the X-SAFEPROTECTION header in a case‑sensitive manner. 
Tools like Burp Suite and HTTP/2‑based requests normalize header casing, which caused the backend to reject otherwise valid requests.
This behavior represents a server‑side implementation flaw, as HTTP headers are defined to be case‑insensitive.

To preserve the exact header casing, HTTP/1.1 was explicitly enforced when sending the request:
```
curl -H "X-SAFEPROTECTION: enNlYW5vb2Zjb3Vyc2U=" \
--http1.1 \
"https://aab37d0e10cb428a8ac87c02cc50abe5.ctf.hacker101.com/api/action.php?act=getemail"
```
With the correct header casing preserved, the backend accepted the request and returned the response.
The response contained JSON with an email address and the flag. <br><br>
<img width="947" height="180" alt="image" src="https://github.com/user-attachments/assets/4e216cf4-9f88-4412-9c6f-79c42fa014f9" />

