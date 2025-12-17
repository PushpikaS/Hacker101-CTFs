# Postbook

- Platform: Hacker101 <br>
- Category: Web  <br>
- Difficulty: Easy  <br>

## Flag0 
Hints:
- The person with username "user" has a very easy password... <br>

Since the hint mentions 'easy password', logged in with username: ```user``` and password: ```password``` which resulted in a successful login and revealed the flag.
<br><br>
<img width="1918" height="817" alt="image" src="https://github.com/user-attachments/assets/812cf571-2fab-4a11-959a-07ec4929efe9" /> <br><br>

## Flag1 
Hints:
- Try viewing your own post and then see if you can change the ID <br>

Edited the first post which shows the id in the url as ```id=3``` - changed the id to ```id=2``` in the url which revealed a private post by the admin. <br>
Saved the post which revealed the flag. <br><br>

<img width="820" height="702" alt="image" src="https://github.com/user-attachments/assets/64ce2a3f-45bc-4953-9c7d-1c524ebb3c39" /> <br><br>

<img width="790" height="706" alt="image" src="https://github.com/user-attachments/assets/6a6695a1-4a48-4d8f-9322-38fbc7db78e8" /> <br><br>

<img width="1513" height="481" alt="image" src="https://github.com/user-attachments/assets/3093262a-504c-4554-a0c7-bc02f40be789" /> <br><br>


## Flag2 
Hints:
- You should definitely use "Inspect Element" on the form when creating a new post <br>

Went to the post creation tab and used Inspect Element. Noticed a hidden element ```<input type="hidden" name="user" value="2">```  <br>
Changed the value in that hidden element to ```value="3"``` and saved the post which revealed the flag. <br> <br>
<img width="1652" height="692" alt="image" src="https://github.com/user-attachments/assets/092d8b05-4511-4909-8c54-0ee84c8ac37a" /> <br><br>
<img width="856" height="443" alt="image" src="https://github.com/user-attachments/assets/88756c0b-4b79-4cb9-a9f8-dde937efc6a5" />  <br> <br>


## Flag3 
Hints:
- 189 * 5 <br>

 189 * 5 = 945 <br>
 Replace the id value in the URL used when Flag 2 was found with ```id=945``` <br>
```https://fa584ccc45294d6d53291724386073ba.ctf.hacker101.com/index.php?page=view.php&success=1&id=945&message=[FLAG_REDACTED]``` <br>
Loading this modified URL reveals the flag.  <br><br>
<img width="1075" height="477" alt="image" src="https://github.com/user-attachments/assets/f7c16e0f-d9cd-44d5-830e-e6522e2fa292" />  <br><br>


## Flag4
Hints:
- You can edit your own posts, what about someone else's? <br>

Visited the admin's post using ```id=2``` in the url as shown from the process to find flag1 and checked the box for  ```Yes, this is my own eyes only!``` and saved the post which revealed the flag. <br><br>
<img width="758" height="691" alt="image" src="https://github.com/user-attachments/assets/7d059b4a-321e-4c14-99a9-800361e6bb62" /> <br><br>
<img width="1502" height="542" alt="image" src="https://github.com/user-attachments/assets/90c9d98e-d209-4fed-96d9-6e66dbcc0e9c" /> <br><br>



## Flag5 
Hints:
- The cookie allows you to stay signed in. Can you figure out how they work so you can sign in to user with ID 1?  <br>

 In Google Chrome -> Dev tools -> Application -> Cookies  <br>
 The ```id``` cookie is the authentication cookie.   <br><br>
<img width="787" height="546" alt="image" src="https://github.com/user-attachments/assets/6a011c08-8557-406a-bc11-d1675efc94c4" />  <br><br>
The ```id``` cookie appeared to be an MD5 hash. To confirm this, an online MD5 hash generator was used. Hashing the value ```2``` produced an MD5 hash that matched the cookie value observed in the browser, suggesting that the cookie was simply an MD5‑encoded user ID.  <br>

Since user ID 1 is commonly reserved for an administrator account, a new MD5 hash was generated for the value 1. Replacing the existing id cookie with this hash and refreshing the page resulted in administrator access, which revealed the flag.  <br><br>
<img width="1751" height="603" alt="image" src="https://github.com/user-attachments/assets/250d6e5b-9b52-45e7-85dc-817add9af0cf" />  <br><br>


## Flag6 
Hints:
- Deleting a post seems to take an ID that is not a number. Can you figure out what it is?  <br>

Inspecting the delete button reveals the following: ```index.php?page=delete.php&amp;id=eccbc87e4b5ce2fe28308fd9f2a7baf3```  <br>
The id parameter is an MD5 hash, not a number. Testing confirms that this value is the MD5 hash of the post ID (for example, md5(3)). <br><br>

<img width="600" height="442" alt="image" src="https://github.com/user-attachments/assets/84243093-9d10-46bd-bfec-d998060c3784" /> <br><br>

By replacing the id value with the MD5 hash of another post’s ID, it is possible to delete posts that do not belong to the current user. <br>

Inspecting the page reveals that the admin post has an ID of 1. Using MD5 hash of 1 in the delete URL and clicking the delete button successfully deletes the admin post and reveals the flag. <br><br>

<img width="871" height="637" alt="image" src="https://github.com/user-attachments/assets/177aab24-2c2f-47b6-ae0b-e419862bff83" />

