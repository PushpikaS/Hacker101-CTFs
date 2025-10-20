# Photo Gallery

- Platform: Hacker101 <br>
- Category: Web  <br>
- Difficulty: Medium  <br>

## Flag0 
Hints:
- Consider how you might build this system yourself. What would the query for fetch look like?
- Take a few minutes to consider the state of the union
- This application runs on the uwsgi-nginx-flask-docker image

Inspected the code and noticed the ```fetch``` endpoint.
<img width="700" height="390" alt="image" src="https://github.com/user-attachments/assets/7c7c9fe6-56d1-4343-be88-48c8fade7951" /> <br><br>

Because the second hint mentioned ```union```, a SQL injection was indicated. For this website, the only way to perform this attack would be through the URL. The third hint referenced ```uwsgi-nginx-flask-docker```; since ```Flask``` commonly runs from ```main.py```, ```main.py``` was identified as the primary attack target.
<br>
```d1ae6902fc23e9b5dbf9e2b75d730fcb.ctf.hacker101.com/fetch?id=4 UNION SELECT 'main.py'``` <br><br>
<img width="1917" height="762" alt="image" src="https://github.com/user-attachments/assets/9d13568d-ffdc-4136-bbf4-48efbc5fa625" />  <br><br>


## Flag1 
Hints:
- I never trust a kitten I can't see
- Or a query whose results I can't see, for that matter

The hint 'query' pointed to a SQL injection vulnerability. The previous location where  ```Flag0``` was placed revealed a database named ```level5```, ```sqlmap``` was then used to enumerate that database: <br>
```sqlmap -u https://d1ae6902fc23e9b5dbf9e2b75d730fcb.ctf.hacker101.com/fetch?id=1 --dump -D level5 --threads=5``` <br><br>
<img width="938" height="283" alt="image" src="https://github.com/user-attachments/assets/52e35e44-cf5c-4599-8b85-ba0a96c56971" /> <br><br>

The flag was revealed after enumeration. <br><br>
<img width="760" height="282" alt="image" src="https://github.com/user-attachments/assets/5972a0a7-9667-4b22-98c8-7e43cae025c1" /> <br><br>


## Flag2 
Hints:
- That method of finding the size of an album seems suspicious
- Stacked queries rarely work. But when they do, make absolutely sure that you're committed
- Be aware of your environment
