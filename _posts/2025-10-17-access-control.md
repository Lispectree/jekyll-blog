---
layout: post
title: "Access control write-up"
date: 2025-10-17
categories: web-security
tags: [access-control, pentesting]
---
# High-level summary
Access control vulnerabilities happen when an application does not properly enforce who can do what. This can let users view or change data, perform actions, or access pages they shouldn't. Understanding these issues helps ensure users only get the permissions they are supposed to have.

## Lab 1 — Unprotected Admin Panel

**What this teaches:** How hidden URLs can still allow unauthorized access.

**Simple beginner walkthrough:**

1. View the lab home page source in your browser or Burp.
2. Look for JavaScript that shows the admin panel URL.
3. Open the admin panel and complete the lab task.

![Access Control Lab 1 Photo 1](/images/access_control_lab1_photo_1.jpg)
This shows that the admin subdomain isn’t found

![Access Control Lab 1 Photo 2](/images/access_control_lab1_photo_2.jpg)
/robots.txt shows hidden subdomains

![Access Control Lab 1 Photo 3](/images/access_control_lab1_photo_3.jpg)
Delete that user that was instructed by the lab

## Lab 2 — User Role Modification

**What this teaches:** How modifying profile data can escalate privileges.

**Simple beginner walkthrough:**

1. Log in and go to your account page.
2. Update your email and observe your role ID in the response.
3. Send the request to Burp Repeater and change the roleid to 2.
4. Forward the request and confirm your new role.
5. Access /admin and complete the lab task.
   
![Access Control Lab 2 Photo 1](/images/access_control_lab2_photo_1.jpg)
The lab information telling you the admin page is accessible to users with a roleid of 2

![Access Control Lab 2 Photo 2](/images/access_control_lab2_photo_2.jpg)
The request and response of the change email request as seen here
Showing your role id

![Access Control Lab 2 Photo 3](/images/access_control_lab2_photo_3.jpg)
Role id can be manipulated by the user to 2 which will give you admin privileges
Enter the admin panel and complete the lab

## Lab 3 — URL-based Access Control Bypass

**What this teaches:** How modifying request headers can bypass front-end restrictions.

**Simple beginner walkthrough:**

1. Try loading /admin and notice you are blocked.
2. Send the request to Burp Repeater, change URL to /, and add X-Original-URL: /invalid.
3. Change the header to /admin and forward the request.
4. Use ?username=carlos in the query string with X-Original-URL: /admin/delete to complete the task.

![Access Control Lab 3 Photo 1](/images/access_control_lab3_photo_1.jpg)
This header highlighted(X-Original-url) shows that the request header Chan be manipulated bypassing restrictions

![Access Control Lab 3 Photo 2](/images/access_control_lab3_photo_2.jpg)
The admin page is assessed and the response contains a link to delete users
Copy the link to delete the user as directed by the lab

![Access Control Lab 3 Photo 3](/images/access_control_lab3_photo_3.jpg)
Delete the user 
And the lab is complete


## Lab 4 — Referer-based Access Control

**What this teaches:** How access rules based on the Referer header can be bypassed.

**Simple beginner walkthrough:**

1. Log in as admin and perform an action on carlos, sending the request to Burp Repeater.
2. Open an incognito window, log in as a non-admin, and try the same request.
3. Copy the non-admin session cookie into Burp, adjust the username to yours, and forward the request.
4. Complete the lab task.

![Access Control Lab 4 Photo 1](/images/access_control_lab4_photo_1.jpg)
The lab instructions

![Access Control Lab 4 Photo 2](/images/access_control_lab4_photo_2.jpg)
This is the request used to upgrade Carlos user
We will copy this and use it for Wiener


We use the path for Wiener and the lab is complete




