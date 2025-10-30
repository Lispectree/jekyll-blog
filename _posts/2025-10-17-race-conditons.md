---
layout: post
title: Race Conditions write-up
date: 2025-10-17
categories: web-security
tags: [race conditions, pentesting]
---
# High-level summary
Race conditions occur when an application does not properly handle multiple operations happening at the same time. Attackers can exploit timing gaps to bypass restrictions, duplicate actions, or manipulate sensitive processes. Understanding these flaws helps developers design systems that safely manage simultaneous actions.

## Lab 1 — Limit Overrun Race Conditions

**What this teaches:** How sending multiple requests at the same time can bypass quantity or usage limits.

**Simple beginner walkthrough:**

1. Log in and add a cheap item to your cart using a discount code.
2. Identify the endpoints for adding items and applying discount codes.
3. In Burp Repeater, duplicate the POST /cart/coupon request into a group with 20 tabs.
4. Send the requests in parallel and observe that the discount is applied multiple times.
5. Add the leather jacket and repeat to reduce the total below your store credit, then complete the purchase.

![Race Condition Lab 1 Photo 1]({{ site.baseurl }}/images/race_lab1_photo_1.jpg)
After logging in it shows you have a promo code


![Race Condition Lab 1 Photo 2]({{ site.baseurl }}/images/race_lab1_photo_2.jpg)
Try and acquire a product well above your balance by trying to make use of the promo code multiple times simultaneously through race conditions


![Race Condition Lab 1 Photo 3]({{ site.baseurl }}/images/race_lab1_photo_3.jpg)
Send all the request in parallel

## Lab 2 — Multi-endpoint Race Conditions

**What this teaches:** How exploiting timing across multiple endpoints can bypass purchase restrictions.

**Simple beginner walkthrough:**

1. Log in and add a gift card to your cart.
2. Identify the POST /cart and POST /cart/checkout endpoints.
3. In Burp Repeater, send the requests in parallel instead of sequentially.
4. Observe if the checkout succeeds even with insufficient funds.
5. Repeat with the leather jacket until the purchase completes successfully.
![Race Condition Lab 2 Photo 1]({{ site.baseurl }}/images/race_lab2_photo_1.jpg)
   The lab


![Race Condition Lab 2 Photo 2]({{ site.baseurl }}/images/race_lab2_photo_2.jpg)
 Send the cart and check out request simultaneously


![Race Condition Lab 2 Photo 3]({{ site.baseurl }}/images/race_lab2_photo_3.jpg)
  


## Lab 3 — Exploiting Time-Sensitive Vulnerabilities

**What this teaches:** How simultaneous requests can produce identical tokens for password resets.

**Simple beginner walkthrough:**

1. Submit a password reset for your account and observe the token in the email.
2. Send multiple POST /forgot-password requests in parallel using different session cookies.
3. Observe when two requests generate identical tokens.
4. Change the username in one request to target another user, like carlos.
5. Use the shared token to reset the other user's password and complete the lab.



