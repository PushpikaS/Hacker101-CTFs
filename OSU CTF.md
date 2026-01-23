# OSU CTF

- Platform: Hacker101 <br>
- Category: Web  <br>
- Difficulty: Moderate  <br>

## Flag0 
Hints:
- Always check the JS on the page for unlinked routes!

The challenge starts with a login page that asks for a username and password. There were no obvious filters or restrictions on the input fields, so SQL injection was tested by entering the same payload in both the username and password field: 
``` admin' or '1'='1 ```

This payload works by turning the login query into a condition that always evaluates as true. Since ```'1'='1``` is always true, the database returns a valid result without checking actual credentials. After submitting the form, authentication was bypassed successfully and access to the dashboard was granted. 

After getting access, the page looked limited. Student names were visible, but they could not be clicked.

<img width="1912" height="840" alt="image" src="https://github.com/user-attachments/assets/6bfb2215-0e3f-493e-b5d0-298c9db5faed" />  <br>

The hint suggested looking deeper, so the JavaScript files loaded by the page were inspected using the browser’s developer tools. Inspected ```app.min.js```

<img width="1918" height="302" alt="image" src="https://github.com/user-attachments/assets/89bb677d-3998-4216-9249-16133576736b" />  <br>

Inside ```app.min.js```, a variable called ```s.admin``` was found. This variable is part of the JavaScript object that controls how the page behaves. In JavaScript, objects like this are often attached to the window object, which means they are globally accessible from the browser console. That is why it can be accessed as ```window.staff.admin``` or simply ```staff.admin```.

```
(function(s, objectName) {
    setupLinks = function() {
        if (s.admin) {
            var sl = document.getElementsByClassName("student-link");
            for (i = 0; i < sl.length; i++) {
                let name = sl[i].innerHTML;
                sl[i].style.cursor = 'pointer';
                sl[i].addEventListener("click", function() {
                    window.location = '/update-' + objectName + '/' + this.dataset.id;
                });
            }
        }
    };
    updateForm = function() {
        var submitButton = document.getElementsByClassName("update-record");
        if (submitButton.length === 1) {
            submitButton[0].addEventListener("click", function() {
                var english = document.getElementById("english");
                english = english.options[english.selectedIndex].value;
                var science = document.getElementById("science");
                science = science.options[science.selectedIndex].value;
                var maths = document.getElementById("maths");
                maths = maths.options[maths.selectedIndex].value;
                var grades = new Set(["A", "B", "C", "D", "E", "F"]);
                if (grades.has(english) && grades.has(science) && grades.has(maths)) {
                    document.getElementById('student-form').submit();
                } else {
                    alert('Grades should only be between A - F');
                }
            });
        }
    };
    setupLinks();
    updateForm();
})(staff, 'student');
```

This variable was being used by the front end to decide whether admin-only actions should be enabled. If ```s.admin``` is ```false```, buttons and links stay disabled. If it is ```true```, extra features become available. This check happens entirely in the browser and not on the server, which is a serious mistake.

To confirm this, the value was checked in the console:

```console.log(window.staff.admin)``` or ```console.log(staff.admin)```

It returned ```false```. Since this is just a JavaScript variable running in the browser, it can be changed manually. Setting it to ```true``` tricks the page into thinking the logged-in user is an admin:

```
staff.admin = true;
setupLinks();
```

The ```setupLinks()``` function re-runs the logic that enables admin-only links. After this, the page updates and student names become clickable, even though no real admin permission was ever given by the server.

<img width="837" height="315" alt="image" src="https://github.com/user-attachments/assets/6450905e-8a03-402d-ae60-e7d919ae2574" />  <br>

Clicking on a student redirects to a URL like:
``` https://d2a549eebd3c50530dc427f7b64e9350.ctf.hacker101.com/update-student/TmFuY2llX0JyZXR0 ```
The value at the end ```TmFuY2llX0JyZXR0``` does not look random. 

<img width="1083" height="492" alt="image" src="https://github.com/user-attachments/assets/1ffc0b8a-fd87-49c4-b6d1-6d2d57e98a2e" />  <br>

Used a tool online to check what type of hash it was and it turned out to be ```Base64```.

<img width="1918" height="581" alt="image" src="https://github.com/user-attachments/assets/6fa20906-fa31-4dab-a1d2-3880314c899b" />  <br>

When decoded from ```Base64```, it turns into: ```Nancie_Brett```

This shows that the application is only hiding student names using encoding, not protecting them. Encoding is reversible and offers no security. To test this weakness, the decoded value was changed to another student name, ```Natasha_Drew```, then encoded back into ```Base64``` and placed into the URL: ```TmF0YXNoYV9EcmV3``` 

```https://d2a549eebd3c50530dc427f7b64e9350.ctf.hacker101.com/update-student/TmF0YXNoYV9EcmV3```

Opening this URL loads an update form for that student. 

<img width="1087" height="515" alt="image" src="https://github.com/user-attachments/assets/8bd26af4-7d5d-4e67-85e2-107fd45aa5b8" />  <br>

All grades were changed to A and the form was submitted. Since the server does not properly check whether the user is allowed to make this change, the update succeeds. <br>
After saving the record, the application displays the flag.  <br>
<img width="1918" height="613" alt="image" src="https://github.com/user-attachments/assets/cd7738b6-206b-4e6c-b2e4-25d20850363d" />


