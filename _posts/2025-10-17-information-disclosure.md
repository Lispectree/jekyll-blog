# High-level summary

Information disclosure vulnerabilities happen when an application accidentally reveals sensitive information. This could include error messages, system details, or hints about access rules. Learning about these helps prevent attackers from using that information maliciously.

## Lab 1 — Error Message Disclosure

**What this teaches:** How entering unusual inputs can show hidden server details.

**Simple beginner walkthrough:**

1. Open a product page and find the request with the productID.
2. Send the request to Burp Repeater.
3. Change the productId to a word like "example" and send it.
4. Look at the error message showing server info.
5. Submit the server version to complete the lab.

![image alt](https://github.com/Lispectree/web-sec/blob/0ba928deb2a195ef8c2dc7ecad1253739f3719d6/web-security-labs/labs/information-disclosure/INFORMATION%20LAB1%20PHOTO1.jpg)
The lab instructions


![image alt](https://github.com/Lispectree/web-sec/blob/337664eac720b6ddd180114eaa84192398b27466/web-security-labs/labs/information-disclosure/INFORMATION%20LAB1%20PHOTO2.jpg)
The number value indicate numbers are attached to the product


![image alt](https://github.com/Lispectree/web-sec/blob/1e4905c8dbfac1cc4560f4890eb4d0df143a5488/web-security-labs/labs/information-disclosure/INFORMATION%20LAB1%20PHOTO3.jpg)
After adding a randomized string 
It brings out errror message and discloses additional information

## Lab 2 — Authentication Bypass via Information Disclosure

**What this teaches (one line):** How headers can reveal ways to bypass access restrictions.

**Simple beginner walkthrough:**

1. Send a GET /admin request and notice access rules.
2. Resend the request with the TRACE method and see the special header.
3. In Burp, set a rule to automatically add X-Custom-IP-Authorization: 127.0.0.1 to all requests.
4. Go to the home page and check admin access.
5. Complete the lab task.


![image alt](https://github.com/Lispectree/web-sec/blob/770296d9912cb56b826cddbb54f09e2f49ac3584/web-security-labs/labs/information-disclosure/INFORMATION%20LAB2%20PHOTO1.jpg)
The lab instructions


![image alt](https://github.com/Lispectree/web-sec/blob/7686607ee70ac1f9a598c21a6b933615774fbaf6/web-security-labs/labs/information-disclosure/INFORMATION%20LAB2%20PHOTO2.jpg)
Using Trace method
It shows in the response that your ip address is being checked
Change it you an internal ip address and use get request to access the admin page


![image alt](https://github.com/Lispectree/web-sec/blob/9489968d80f297eeb94870dc9ea1faa6f807610f/web-security-labs/labs/information-disclosure/INFORMATION%20LAB2%20PHOTO3.jpg)
Delete the user and solve the lab





