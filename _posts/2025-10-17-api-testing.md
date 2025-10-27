---
layout: post
title:  API Testing  write-up
date: 2025-10-17
categories: web-security
tags: [api testing, pentesting]
---

# High-level summary
API vulnerabilities occur when web applications expose endpoints that can be accessed in unexpected ways. Attackers can manipulate these endpoints to retrieve data, modify resources, or perform actions they shouldn't be allowed to. This lab set teaches beginners how to explore and test API endpoints safely.

## Lab 1 — Exploiting an API endpoint using documentation

**What this teaches:**
Learn how exposed API documentation can reveal endpoints and actions.

**Simple beginner walkthrough:**

1. Log in to the application and update your email.
2. Find the API request for updating your email and send it to Burp Repeater.
3. Try removing parts of the URL to discover the API documentation.
4. Open the interactive documentation in your browser and locate the DELETE endpoint.
5. Use the DELETE endpoint to remove the user carlos to solve the lab.

![API Testing Lab 1 Photo 1]({{ site.baseurl }}/images/api_testing_lab1_photo_1.jpg)
Use the update email function


![API Testing Lab 1 Photo 2]({{ site.baseurl }}/images/api_testing_lab1_photo_2.jpg)
Monitor the request and remove any other requests attached to the api to see that api documentation


![API Testing Lab 1 Photo 3]({{ site.baseurl }}/images/api_testing_lab1_photo_3.jpg)
Follow the api documentation of the delete request to delete the user instructed by the lab


## Lab 2 — Finding and exploiting an unused API endpoint

**What this teaches:**
Learn how to identify hidden or unused API endpoints and manipulate them.

**Simple beginner walkthrough:**

1. Click on a product and locate its API request in Burp HTTP history.
2. Send the request to Burp Repeater.
3. Change the request method to PATCH and add the Content-Type header as application/json.
4. Add a JSON body with {"price":0} to set the product price to zero.
5. Reload the product page, add it to your basket, and place the order to solve the lab.

![API Testing Lab 2 Photo 1]({{ site.baseurl }}/images/api_testing_lab2_photo_1.jpg)
This shows the request for a product we are trying to purchase

![API Testing Lab 2 Photo 2]({{ site.baseurl }}/images/api_testing_lab2_photo_2.jpg)
Changing the request to PATCH 
It shows that a json file is allowed to be attached to the request

![API Testing Lab 2 Photo 3]({{ site.baseurl }}/images/api_testing_lab2_photo_3.jpg)
We take advantage of that and change the price to 0 using the json file

![API Testing Lab 2 Photo 4]({{ site.baseurl }}/images/api_testing_lab2_photo_4.jpg)
As seen in the lab that price is now 
$0
Complete the lab by acquiring the product


## Lab 3 — Exploiting server-side parameter pollution in a REST URL

**What this teaches:**
Learn how API requests can be manipulated to access sensitive data through URL parameters.

**Simple beginner walkthrough:**

1. Trigger a password reset for the administrator account and send the request to Burp Repeater.
2. Modify the username parameter to explore different paths and API versions.
3. Identify a valid field parameter to retrieve the password reset token.
4. Use the reset token in the password reset URL to set a new password.
5. Log in as administrator and delete carlos to solve the lab.

![API Testing Lab 3 Photo 1]({{ site.baseurl }}/images/api_testing_lab3_photo_1.jpg)
   The request of the /forgotpassword
Username parameter is edited using path traversal to show a file that can be used to access any field

![API Testing Lab 3 Photo 2]({{ site.baseurl }}/images/api_testing_lab3_photo_2.jpg)
 The path to show the reset token of the administrator

![API Testing Lab 3 Photo 3]({{ site.baseurl }}/images/api_testing_lab3_photo_3.jpg)
  The rest token

![API Testing Lab 3 Photo 4]({{ site.baseurl }}/images/api_testing_lab3_photo_4.jpg)
 Input the rest token into the url and you can change the password and access the administrator account to delete the username instructed
  

 

