# High-level summary

Access control vulnerabilities happen when an application does not properly enforce who can do what. This can let users view or change data, perform actions, or access pages they shouldn't. Understanding these issues helps ensure users only get the permissions they are supposed to have.

## Lab 1 — Unprotected Admin Panel

**What this teaches:** How hidden URLs can still allow unauthorized access.

**Simple beginner walkthrough:**

1. View the lab home page source in your browser or Burp.
2. Look for JavaScript that shows the admin panel URL.
3. Open the admin panel and complete the lab task.

   ![image alt](https://github.com/Lispectree/web-sec/blob/8dc05cf749b36defe855a82cd955375c129dd404/labs/sql-injection/access%20control%20lab1%20photo%201.jpg)
This shows that the admin subdomain isn’t found

![image alt](https://github.com/Lispectree/web-sec/blob/120bbc6a0faf6eb4d872b44978d8007ddf56a8c0/labs/sql-injection/access%20control%20lab1%20photo%202.jpg)
/robots.txt shows hidden subdomains

![image alt](https://github.com/Lispectree/web-sec/blob/5226e16c869b8109f5f226568b53f2eb2428aab6/labs/sql-injection/access%20control%20lab1%20photo%203.jpg)
Delete that user that was instructed by the lab

## Lab 2 — User Role Modification

**What this teaches:** How modifying profile data can escalate privileges.

**Simple beginner walkthrough:**

1. Log in and go to your account page.
2. Update your email and observe your role ID in the response.
3. Send the request to Burp Repeater and change the roleid to 2.
4. Forward the request and confirm your new role.
5. Access /admin and complete the lab task.
   
 ![image alt](https://github.com/Lispectree/web-sec/blob/8aaa048b53f1f17d4c90ada1e3d3e972bec0ee7e/labs/sql-injection/access%20control%20lab2%20photo%201.jpg)
The lab information telling you the admin page is accessible to users with a roleid of 2

![image alt](https://github.com/Lispectree/web-sec/blob/2383ab4def01407b5c101aea3d76ff93469438cd/labs/sql-injection/access%20control%20lab2%20photo%202.jpg)
The request and response of the change email request as seen here
Showing your role id

![image alt](https://github.com/Lispectree/web-sec/blob/2383ab4def01407b5c101aea3d76ff93469438cd/labs/sql-injection/access%20control%20lab2%20photo%203.jpg)
Role id can be manipulated by the user to 2 which will give you admin privileges
Enter the admin panel and complete the lab

## Lab 3 — URL-based Access Control Bypass

**What this teaches:** How modifying request headers can bypass front-end restrictions.

**Simple beginner walkthrough:**

1. Try loading /admin and notice you are blocked.
2. Send the request to Burp Repeater, change URL to /, and add X-Original-URL: /invalid.
3. Change the header to /admin and forward the request.
4. Use ?username=carlos in the query string with X-Original-URL: /admin/delete to complete the task.

![image alt](https://github.com/Lispectree/web-sec/blob/3994a1361ce0cfdcd8db6998ae1762aeb9dc9586/labs/sql-injection/access%20control%20lab3%20photo%201.jpg)
This header highlighted(X-Original-url) shows that the request header Chan be manipulated bypassing restrictions

![image alt](https://github.com/Lispectree/web-sec/blob/4f9582632080c119723e56c8fddb34f7feaaf9dd/labs/sql-injection/access%20control%20lab3%20photo%202.jpg)
The admin page is assessed and the response contains a link to delete users
Copy the link to delete the user as directed by the lab

![image alt](https://github.com/Lispectree/web-sec/blob/95ee06fdcb77b2869423bc6ee8fb419bb905fd3a/labs/sql-injection/access%20control%20lab3%20photo%2013.jpg)
Delete the user 
And the lab is complete


## Lab 4 — Referer-based Access Control

**What this teaches:** How access rules based on the Referer header can be bypassed.

**Simple beginner walkthrough:**

1. Log in as admin and perform an action on carlos, sending the request to Burp Repeater.
2. Open an incognito window, log in as a non-admin, and try the same request.
3. Copy the non-admin session cookie into Burp, adjust the username to yours, and forward the request.
4. Complete the lab task.

![image alt](https://github.com/Lispectree/web-sec/blob/f30595c7647f57683c55bd8e5870f46f37c9ebf0/labs/sql-injection/access%20control%20lab4%20photo%201.jpg)
The lab instructions

![image alt](https://github.com/Lispectree/web-sec/blob/23f070ecaf7b886074ade1b0df9d7055215ebca1/labs/sql-injection/access%20control%20lab4%20photo%202.jpg)
This is the request used to upgrade Carlos user
We will copy this and use it for Wiener

![image alt](https://github.com/Lispectree/web-sec/blob/abe6b6bedb6120c057803c9c1d41c9318a52ee27/labs/sql-injection/access%20control%20lab4%20photo%203.jpg)
We use the path for Wiener and the lab is complete




