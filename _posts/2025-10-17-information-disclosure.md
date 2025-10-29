---
layout: post
title: "Information Disclosure write-up"
date: 2025-10-17
categories: web-security
tags: [information disclosure, pentesting]
---
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

![Information Disclosure Lab 1 Photo 1]({{ site.baseurl }}/images/information_disclosure_lab1_photo_1.jpg)
The lab instructions


![Information Disclosure Lab 1 Photo 2]({{ site.baseurl }}/images/information_disclosure_lab1_photo_1.jpg)
The number value indicate numbers are attached to the product


![Information Disclosure Lab 1 Photo 3]({{ site.baseurl }}/images/information_disclosure_lab1_photo_3.jpg)
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


![Information Disclosure Lab 2 Photo 1]({{ site.baseurl }}/images/information_disclosure_lab2_photo_1.jpg)
The lab instructions


![Information Disclosure Lab 2 Photo 2]({{ site.baseurl }}/images/information_disclosure_lab2_photo_2.jpg)
Using Trace method
It shows in the response that your ip address is being checked
Change it you an internal ip address and use get request to access the admin page


![Information Disclosure Lab 2 Photo 3]({{ site.baseurl }}/images/information_disclosure_lab2_photo_3.jpg)
Delete the user and solve the lab





