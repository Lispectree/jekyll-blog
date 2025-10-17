# OS Command Injection — Simple Walkthroughs

**High-level summary of OS Command Injection**

OS Command Injection happens when a website takes text you give it and runs that text as a command on the server. If the site doesn’t check or limit that text, someone can add extra commands to be executed — for example, to find the username the server runs as, or to write command output to a file and read it later. This is dangerous because it lets an attacker run commands on the server itself.

---

## Lab 1 — OS command injection (simple case)

**What this teaches:**
How adding a command separator and a small system command to an input can make the server run that command and return its output.

**Simple beginner walkthrough:**

1. Open the page that checks stock and use Burp Suite to intercept the request it sends.
2. Edit the `storeID` value so it contains a command, for example: `1|whoami`. The `|` means “run this command too.”
3. Send the edited request and look at the response — if it contains a username (the result of `whoami`), the server executed your command.
![image alt](https://github.com/Lispectree/web-sec/blob/58a7c801f1d19cdeacbc32cab91cf8bffca70a6a/web-security-labs/labs/os-command-injection/OS%20LAB1%20PHOTO1.jpg)
Lab instructions


![image alt](https://github.com/Lispectree/web-sec/blob/0f72540f9e44f69416517689bf768100d6745108/web-security-labs/labs/os-command-injection/OS%20LAB1%20PHOTO2.jpg)
The command to generate the use

---

## Lab 2 — Blind OS command injection with output redirection

**What this teaches (one line):**
How to run a command that writes its output to a file, then request that file to view the output when the site doesn’t return command output directly.

**Simple beginner walkthrough:**

1. Intercept the request that submits feedback and edit the `email` field to include a command that writes output to a file, for example:
   `email=||whoami>/var/www/images/output.txt||` — this runs `whoami` and saves the output to `output.txt`.
2. Send that feedback request so the server executes the command and writes the file.
3. Intercept the request that loads a product image, change the `filename` parameter to `output.txt`, and send it. The response will contain the contents of `output.txt` (the command output), showing the injection worked.

![image alt](https://github.com/Lispectree/web-sec/blob/a88708c338cf118b53e8e2ea9df83596ae5cd5fc/web-security-labs/labs/os-command-injection/OS%20LAB2%20PHOTO1.jpg)
The email section is vulnerable to command injection


![image alt](https://github.com/Lispectree/web-sec/blob/835726c849f8d1178228f53565698feeef773b77/web-security-labs/labs/os-command-injection/OS%20LAB2%20PHOTO2.jpg)
But it doesn’t disclose anything 
So we test it by telling the server to sleep for 5sec before responding


![image alt](https://github.com/Lispectree/web-sec/blob/8018894fefcd1857c882fe282ee117d99a2f7858/web-security-labs/labs/os-command-injection/OS%20LAB2%20PHOTO3.jpg)
We that use redirection to disclose the whoami command inside a txt file


---

