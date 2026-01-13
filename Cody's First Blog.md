# Cody's First Blog

- Platform: Hacker101 <br>
- Category: Web  <br>
- Difficulty: Medium  <br>

## Flag0 
Hints:
- What was the first input you saw?
- Figuring out what platform this is running on may give you some ideas
- Code injection usually doesn't work

The page source mentioned that PHP ```include()``` was used, which suggested that page content was loaded dynamically on the server.  <br> 

Viewing the page source showed a commented‑out link containing ```page=admin.auth.inc.```   <br><br>
<img width="841" height="591" alt="image" src="https://github.com/user-attachments/assets/f668aa7e-031c-4f7a-a148-bea24f098cfa" />  <br>

This suggested that the application uses PHP ```include()``` to load pages dynamically. Since PHP executes code when using ```include()```, it indicated that injected PHP code could run on the server.  <br>
Based on this, PHP code was injected to echo the predefined FLAG0 constant, which revealed the flag. <br>

```
php
<?php echo ‘FLAG0’; ?>
```
 <br> 
<img width="1918" height="685" alt="image" src="https://github.com/user-attachments/assets/d41a1bf8-9ba6-41b5-ac04-a508aa159344" />

<img width="826" height="215" alt="image" src="https://github.com/user-attachments/assets/79770e30-1ff7-45aa-bb33-4511dc1e0700" />


## Flag1 
Hints:
- Make sure you check everything you're provided
- Unused code can often lead to information you wouldn't otherwise get
- Simple guessing might help you out

 
Given the clue found by viewing the page source — the commented‑out link ```<!--<a href="?page=admin.auth.inc">Admin login</a>-->```  <br>
It indicated that an admin page exists and is loaded using the page parameter.  <br> 
Visiting ```https://015360af24deb642acf05bcf6d2034e7.ctf.hacker101.com/?page=admin.auth.inc``` using ```?page=admin.auth.inc``` led to the admin page.  <br> <br>
<img width="917" height="632" alt="image" src="https://github.com/user-attachments/assets/d7cf97f3-298c-4cd0-8b26-6459af78e7a2" />  <br> 

Since authentication was required and random credentials did not work, simple guessing was used. Removing ```auth``` and changing the URL to ```page=admin.inc``` bypassed authentication and revealed the flag. <br>  <br>
<img width="1102" height="847" alt="image" src="https://github.com/user-attachments/assets/fa40fff5-f149-4f74-9044-d33f620be08d" />


## Flag2 
Hints:
- Read the first blog post carefully
- We talk about this in the Hacker101 File Inclusion Bugs video
- Where can you access your own stored data?
- Include doesn't just work for filenames

Since comments can be submitted and approved, and admin access was gained through the previous process used to find FLAG1, it was already observed that PHP code injected into comments could be executed by the server. <br>

To view the application from the backend perspective, the page parameter was tested with a local path: ```?page=http://localhost/index```  <br>

This confirmed that local files could be accessed through the vulnerable ```include()```.  <br>

The file ```index.php``` was targeted because it is the default entry point of most PHP applications.  <br>

Using the previously identified PHP injection point, the following payload was submitted as a comment:   <br>
```<?php echo readfile("index.php"); ?>```  <br>

After approving the comment from the admin panel, the contents of index.php were displayed. Upon checking the page source of the output, a hidden comment containing FLAG2 was revealed.  <br>

<img width="1918" height="795" alt="image" src="https://github.com/user-attachments/assets/e17862a5-6bc9-41f0-ab00-442d7ad211c7" />

<img width="1110" height="848" alt="image" src="https://github.com/user-attachments/assets/1ee8e5e7-f779-47dc-a5f8-f36a24f87a11" />

