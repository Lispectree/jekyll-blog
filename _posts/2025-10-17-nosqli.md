# High-level summary

NoSQL Injection (NoSQLi) happens when an application improperly handles user input in queries against a NoSQL database. Attackers can manipulate queries to bypass filters, retrieve sensitive data, or escalate privileges. Understanding NoSQLi helps developers prevent injection attacks in modern database systems.

## Lab 1 — Detecting NoSQL Injection

**What this teaches:** How to identify vulnerabilities in user-controlled query parameters.

**Simple beginner walkthrough:**

1. Access the lab and click on a product category filter, intercept the request in Burp.
2. Send the request to Repeater and insert a ' character in the category parameter to check for errors.
3. Test boolean conditions (false and true) in the category parameter and URL-encode them.
4. Submit a condition that always evaluates to true and observe that additional products are returned.
5. Open the response in the browser to verify you can see unreleased products.

![image alt](https://github.com/Lispectree/web-sec/blob/253d2075a3f816d1f91a4a6ace76704293517a3a/web-security-labs/labs/nosqli/NOSQLI%20LAB1%20PHOTO1.jpg)
The error message after adding double quotes


![image alt](https://github.com/Lispectree/web-sec/blob/92031feb56f1238c27190f29230c012678466d25/web-security-labs/labs/nosqli/NOSQLI%20LAB1%20PHOTO2.jpg)
After adding a Boolean expression to release all the product including the unreleased one

## Lab 2 — Exploiting NoSQL Injection to Extract Data

**What this teaches:** How to use NoSQLi to enumerate sensitive data, such as passwords.

**Simple beginner walkthrough:**

1. Log in as wiener\:peter and intercept the GET /user/lookup request in Burp.
2. Send the request to Repeater and insert a ' character in the user parameter to check for errors.
3. Test boolean conditions to retrieve account details for true vs. false scenarios.
4. Determine the administrator password length by adjusting the payload in the user parameter.
5. Send the request to Intruder, configure a Cluster bomb attack with positions for index and letter, and start the attack.
6. Observe the correct characters for each position to reconstruct the administrator password.
7. Log in as the administrator using the enumerated password to solve the lab.

![image alt](https://github.com/Lispectree/web-sec/blob/955564d9fd1f8c0d8b57fd2bfef25a0ee3ff2d45/web-security-labs/labs/nosqli/NOSQLI%20LAB2%20PHOTO1.jpg)
The adding of double quotation shows presence of database vulnerability


![image alt](https://github.com/Lispectree/web-sec/blob/c8017169d1a4ff85060d842b3d80eedafde65d17/web-security-labs/labs/nosqli/NOSQLI%20LAB2%20PHOTO2.jpg)
And intruder attack to determine the number of characters in the password


![image alt](https://github.com/Lispectree/web-sec/blob/55bbf582b7e6885e34c179380e9d8ea241958f1a/web-security-labs/labs/nosqli/NOSQLI%20LAB2%20PHOTO3.jpg)
Password letter written out one by one


