# High-level summary

Server-side template injection (SSTI) occurs when user-supplied input is rendered inside a server-side template without proper sanitization. SSTI can allow attackers to execute arbitrary code, access sensitive objects, or manipulate server-side logic. These labs demonstrate SSTI in multiple template engines: ERB (Ruby), Freemarker (Java), Handlebars, and Django, showing how attackers can exploit template engines to perform destructive or sensitive operations safely in a lab environment.

---

## Lab 1 — Basic server-side template injection (ERB / Ruby)

**What this teaches:** How unsanitized template input can be used to execute system commands in ERB templates.

**Simple beginner walkthrough:**

1. Open the lab and view the first product details.
2. Observe that a `message` parameter is rendered in the template. Test with `<%= 7*7 %>` to confirm that expressions are evaluated.
3. From Ruby documentation, discover that the `system()` method can execute OS commands.
4. Construct the payload `<%= system("rm /home/carlos/morale.txt") %>` and URL-encode it.
5. Add the payload to the `message` parameter in the URL:
   `https://YOUR-LAB-ID.web-security-academy.net/?message=<%25+system("rm+/home/carlos/morale.txt")+%25>`
6. Load the URL to execute the command and solve the lab.
![image alt](https://github.com/Lispectree/web-sec/blob/f3b5431ec59b77905e0167b5eb2fcec0b9553971/web-security-labs/labs/server-side-template-injection/SSTI%20LAB1%20PHOTO1.jpg)
Notice that OS commands injected to the query can be executed


![image alt](https://github.com/Lispectree/web-sec/blob/3b0b1bac453e880618c0e12bbb8a2120b3f49ac8/web-security-labs/labs/server-side-template-injection/SSTI%20LAB1%20PHOTO2.jpg)
Input a command to delete a user file



## Lab 2 — Server-side template injection using documentation (Freemarker / Java)

**What this teaches:** How reading template engine documentation can guide the creation of SSTI exploits in Freemarker templates.

**Simple beginner walkthrough:**

1. Log in and edit a product description template.
2. Identify Freemarker syntax using `${someExpression}`.
3. Study the documentation for the dangerous `new()` built-in and locate the `Execute` class to run OS commands.
4. Construct the payload:
   `<#assign ex="freemarker.template.utility.Execute"?new()> ${ ex("rm /home/carlos/morale.txt") }`
5. Save the template and view the product page. The command executes and the lab is solved.

![image alt](https://github.com/Lispectree/web-sec/blob/564e8b26e65374170a1ccc6eec3cd54e546ff492/web-security-labs/labs/server-side-template-injection/SSTI%20LAB2%20PHOTO1.jpg)
The template
But we don’t know the documentation being used


![image alt](https://github.com/Lispectree/web-sec/blob/37eb03c3d2b776e40e6d8c3ff307b83c303ef87e/web-security-labs/labs/server-side-template-injection/SSTI%20LAB2%20PHOTO2.jpg)
An error discloses it is freemarker


![image alt](https://github.com/Lispectree/web-sec/blob/d685ff20541e5c9f56c9add35f4be23429ebf185/web-security-labs/labs/server-side-template-injection/SSTI%20LAB2%20PHOTO3.jpg)
We remove the file as instructed in the lab


## Lab 3 — SSTI in an unknown template language with documented exploit (Handlebars)

**What this teaches:** How to exploit SSTI in an unfamiliar template engine by adapting a publicly documented exploit.

**Simple beginner walkthrough:**

1. Log in and edit a product description template.
2. Introduce an invalid template expression to detect the engine. Errors hint that Handlebars is in use.
3. Adapt the documented exploit by @Zombiehelp54 to execute system commands.
4. URL-encode the payload and set it as the `message` parameter in the URL.
5. Load the URL to execute the command (`rm /home/carlos/morale.txt`) and solve the lab.

![image alt](https://github.com/Lispectree/web-sec/blob/853fc0ce8ae45a03b3a60c4126dd44940fdab95c/web-security-labs/labs/server-side-template-injection/SSTI%20LAB3%20PHOTO1.jpg)
Brute force to find with type of template is being used


![image alt](https://github.com/Lispectree/web-sec/blob/a6134526b6f766f240db44e2be343074ccd88a7e/web-security-labs/labs/server-side-template-injection/SSTI%20LAB3%20PHOTO2.jpg)
An verbose error shows it is handlebars


---

## Lab 4 — SSTI with information disclosure (Django)

**What this teaches:** How SSTI can be used to access sensitive framework objects, such as Django’s `settings.SECRET_KEY`.

**Simple beginner walkthrough:**

1. Log in and edit a product description template.
2. Introduce an invalid template expression (e.g., `$ {{<%[%'"}}% \`) to detect Django.
3. Use `{% debug %}` in the template to enumerate accessible objects. Notice the `settings` object.
4. Access the secret key using `{{settings.SECRET_KEY}}`.
5. Save the template and submit the rendered key to solve the lab.


![image alt](https://github.com/Lispectree/web-sec/blob/5366493f37bedf9eb7fb2485eac0a6e4a4dd074c/web-security-labs/labs/server-side-template-injection/SSTI%20LAB4%20PHOTO1.jpg)
Verbose error discloses it is using Django


![image alt](https://github.com/Lispectree/web-sec/blob/54c17dddd5d1b153a491c1f987c82da27ffaf3eb/web-security-labs/labs/server-side-template-injection/SSTI%20LAB4%20PHOTO2.jpg)
Access the secret key


