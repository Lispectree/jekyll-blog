# Authentication Vulnerabilities — Simple Walkthroughs

**High-level summary of authentication vulnerabilities**

Authentication is how a website checks who you are (usually with a username and password). Authentication vulnerabilities are mistakes that let someone pretend to be another user or get into accounts they shouldn't. These can include revealing whether a user exists, weak reset logic, predictable tokens, and session problems. The two labs below show two common authentication mistakes and how they can be demonstrated in a safe lab.

---

## Lab 1 — Username enumeration via different responses

**What this teaches:**
How a website’s different messages for login can reveal which usernames actually exist.

**Simple beginner walkthrough:**

1. Open the site’s login page and start Burp Suite so you can capture the login message.
2. In Burp’s HTTP history, find the `POST /login` request and send it to Intruder. Mark the username field as the spot to change and leave the password alone.
3. Run a Sniper attack using a list of candidate usernames. Intruder will try each name automatically.
4. When the attack finishes, sort results by response length and compare messages. Most responses say “Invalid username,” but a longer response saying “Incorrect password” indicates a real username. Note it from the Payload column.
5. Replace the username with that real user, set the password field as the payload position, and run a password list. If one request returns a `302` (redirect) while others return `200`, the `302` likely indicates a successful login. Use the credentials to log in and access the account page.
![image alt](https://github.com/Lispectree/web-sec/blob/e45d210d4c567c15095848a54cbce550f5835d21/web-security-labs/labs/authentication/AUTH%20Lab1%20Photo1.jpg)
After trying a random username and password you get invalid username response

![image alt](https://github.com/Lispectree/web-sec/blob/7a0062254ae4c5b5a76e8361a62f2eea927b3b13/web-security-labs/labs/authentication/AUTH%20Lab1%20Photo2.jpg)
After using the world list provided in the lab
For username enumeration 
One of the responses doesn’t show invalid username indicating the username is correct

![image alt](https://github.com/Lispectree/web-sec/blob/cea709aef87c24b9c6558be349a0a0a796f90ed4/web-security-labs/labs/authentication/AUTH%20Lab1%20Photo3.jpg)
Next is to use the password worldlist for the username
The one that brings out a status code of 302
Is the correct password
You can now login and solve the lab



## Lab 2 — Password reset broken logic (token not checked)

**What this teaches :**
If the password reset token isn’t validated when submitting a new password, an attacker can reset someone else’s password by changing the username in the request.

**Simple beginner walkthrough:**

1. Click the "Forgot your password?" link on the site and request a password reset for your own account. Use the lab’s email client to open the reset email and follow the link to set a new password.
2. While doing this, capture the reset requests in Burp (look for `POST /forgot-password?temp-forgot-password-token`). Notice the reset token appears in the reset email URL and the form that submits the new password.
3. Send the POST request that submits the new password to Burp Repeater. Delete the token value from both the URL and the request body and resend — if the password still changes, the token is not being checked.
4. Request another reset for your account to get a fresh flow, capture the `POST` request again, remove the token value, and change the username field to another user (for example, `carlos`). Set a new password you choose and send the request.
5. Try logging into that other user’s account (e.g., Carlos) with the new password. If it works, the reset logic is broken and allowed you to change someone else’s password without a valid token.

![image alt](https://github.com/Lispectree/web-sec/blob/f09eb5088fbdbb437656715644a42cfda65060c9/web-security-labs/labs/authentication/AUTH%20Lab2%20Photo1.jpg)
The forgot password function of the website

![image alt](https://github.com/Lispectree/web-sec/blob/a4690360a2b63db55abdbafa3ab1721b586f0244/web-security-labs/labs/authentication/AUTH%20Lab2%20Photo2.jpg)
It contains a password token that is cross checked by a client controlled input
Change the two to any random value and change the username to the account the lab instructed you to log into


