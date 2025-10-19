# Micro-CMS v1 
- Platform: Hacker101
- Category: Web 
- Difficulty: Easy 

## Flag0 - Stored XSS via Page Content
Hints:
- Try creating a new page
- How are pages indexed?
- Look at the sequence of IDs
- If the front door doesn't open, try the window
- In what ways can you retrieve page contents?

A stored XSS payload was inserted into the page body, which revealed the flag. <br>
```hello<script>alert(1);</script>``` <br><br>
<img width="678" height="501" alt="image" src="https://github.com/user-attachments/assets/91192354-40d5-4053-8269-99d0196d6b76" /> <br><br>

<img width="1203" height="325" alt="image" src="https://github.com/user-attachments/assets/1fe9cfbc-2faf-400c-9bf8-0e0afe04a403" /> <br>

Remediation: <br>
Sanitize all user input before storing, and enforce a strict Content Security Policy (CSP).
<br><br>


## Flag1 - SQLi via URL Tampering
Hints:
- Make sure you tamper with every input
- Have you tested for the usual culprits? XSS, SQL injection, path injection
- Bugs often occur when an input should always be one type and turns out to be another
- Remember, form submissions aren't the only inputs that come from browsers

Hints recommended tampering with every input and remembering that browser inputs are not limited to form fields. The sequence/ID hint encouraged checking edit URLs when the page id is exposed. <br>
The edit path for a page ID was manipulated by appending a quote character ```'``` to the URL which revealed the flag. <br>
```/page/edit/7'``` <br><br>
<img width="797" height="163" alt="image" src="https://github.com/user-attachments/assets/fd648981-dfd0-4aaf-a68e-d6c88824f215" /> <br>

Remediation: <br>
Sanitize all input, use prepared statements for database queries, and enforce proper access control.
<br><br>


## Flag2 - Authorization Bypass on Edit Page
Hints:
- Sometimes a given input will affect more than one page
- The bug you are looking for doesn't exist in the most obvious place this input is shown

Hints noted that an input may affect more than one page and that the bug may be visible in an unexpected place. <br>
Pages were enumerated sequentially; when page 4 returned ```403 Forbidden``` on normal view, the corresponding edit endpoint was checked: ```/page/edit/4```. 
<br><br>
<img width="982" height="202" alt="image" src="https://github.com/user-attachments/assets/1eb99600-2c5a-4623-a732-1d654fc26b16" />  <br><br>

The edit endpoint for page 4 revealed the flag.  <br><br>
<img width="842" height="297" alt="image" src="https://github.com/user-attachments/assets/a0aaed2a-734a-438f-ac55-c5895495e507" /> <br>

Remediation: <br>
Enforce strict authorization checks on all pages and endpoints.
<br><br>


## Flag3 - DOM XSS via Button Injection
Hints:
- Script tags are great, but what other options do you have?

The hint asked to consider alternatives to ```<script>``` tags; this suggested interactive elements or event handlers. <br>
An interactive element (a button) was saved on an editable page and then viewed. <br>
```<button>onclick=alert(‘xss’)>click</button>``` <br><br>
<img width="712" height="493" alt="image" src="https://github.com/user-attachments/assets/b8aa2b31-e69b-4256-ac6a-3da578f45ba9" /> <br><br>

<img width="1217" height="367" alt="image" src="https://github.com/user-attachments/assets/87a09f49-d39c-4edd-9299-dfa5ba13beba" /> <br><br>

The flag was discovered by inspecting the page source where the injected control and flag were present. <br><br>
<img width="1440" height="403" alt="image" src="https://github.com/user-attachments/assets/b0298be4-6382-423d-abe5-ec815b65facf" /> <br>

Remediation: <br>
Sanitize all attributes and event handlers, encode output, and enforce a strict CSP.
<br><br>
