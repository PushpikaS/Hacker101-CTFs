# Encrypted Pastebin - IN PROGRESS / NOT COMPLETED

- Platform: Hacker101 <br>
- Category: Web, Crypto  <br>
- Difficulty: Hard  <br>

## Flag0 
Hints:
- What are these encrypted links?
- Encodings like base64 often need to be modified for URLs. Thanks, HTTP
- What is stopping you from modifying the data? Not having the key is no excuse

The post creation endpoint was used via the site's form. <br><br>
<img width="835" height="348" alt="image" src="https://github.com/user-attachments/assets/ab88c664-8a3c-49a3-a96f-2fdbd417efb0" /> <br><br>

<img width="565" height="180" alt="image" src="https://github.com/user-attachments/assets/52e11070-cbcd-4e47-9ff7-719e3ce1c4cd" /> <br><br>

Appending a letter ```a``` to the ```post``` parameter of the URL revealed the flag. <br>
```https://fa68b684736fccf47e93734ca3627d0b.ctf.hacker101.com/?post=!4O!Hw3HZk9facTz2xZKPg-SwD5xjneuRj8r8MzwcXeUcnBVn1NlsEjUXppO6hHzk4NQqSsnip3D3wfW31Mus6aRnUeGMIxEjjEq25Z6-u9a0Q0t10CbUTmHWMqI6qCKX2c!SEoC0Cmu3fM0z8HAt5txw7LGfhW0B3EWhf7HzQTqQCyrWoz5kzROx1RBjnn-c9LtXOrh5unORcg!3KJ2LSQ~~``` <br><br>
<img width="1022" height="291" alt="image" src="https://github.com/user-attachments/assets/95737794-3941-4585-954e-764a4649c00b" /> <br>

Remediation: <br>
Validate and canonicalize POST parameters server‑side; reject or canonicalize malformed tokens and enforce strict token format checks before using them to fetch sensitive data.
<br><br>


## Flag1 
Hints:
- We talk about this in detail in the Hacker101 Crypto Attacks video
- Don't think about this in terms of an attack against encryption; all you care about is XOR

```https://fa68b684736fccf47e93734ca3627d0b.ctf.hacker101.com/?post=a```
<img width="1048" height="322" alt="image" src="https://github.com/user-attachments/assets/9a2ecffd-2044-41be-a595-f9e7402af766" />


## Flag2 
Hints:
- Remember: XOR
- Sometimes all it takes is toggling a few bits in the right place


  
## Flag3 
Hints:
- You are on your own for this one. Good luck and have fun!
