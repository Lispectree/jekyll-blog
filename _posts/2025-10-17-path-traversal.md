---
layout: post
title: Path Traversal write-up
date: 2025-10-17
categories: web-security
tags: [path traversal, pentesting]
---
# High-level summary
Path Traversal happens when a website lets you request files by name (for example, images). If the site doesn’t carefully check the filename you give it, you can ask for files outside the intended folder — including sensitive system files. Attackers use special filename text (like `../`) to move up the folder structure and read files the server shouldn’t reveal.

---

## Lab 1 — File path traversal (simple case)

**What this teaches :**
How a basic `../` sequence in a filename can let you read files outside the intended folder.

**Simple beginner walkthrough:**

1. Use Burp Suite to capture the request that fetches a product image.
2. Edit the filename parameter to: `../../../etc/passwd`.
3. Send the modified request and check the response — if it contains `/etc/passwd` contents, the server returned a file it shouldn't.
![Path Traversal Lab 1 Photo 1]({{ site.baseurl }}/images/path_lab1_photo_1.jpg) 
This request gets a jpg file from the server


![Path Traversal Lab 1 Photo 2]({{ site.baseurl }}/images/path_lab1_photo_2.jpg)  
Using path traversal we get the contents of etc/passwd


## Lab 2 — Traversal sequences stripped non-recursively

**What this teaches (one line):**
How naive filters that look for exact `../` sequences can be bypassed by slightly altered patterns.

**Simple beginner walkthrough:**

1. Capture the product image request in Burp.
2. Replace the filename value with: `....//....//....//etc/passwd`.
3. Send the request; if `/etc/passwd` is returned, the simple filter didn’t catch the altered traversal string.


![Path Traversal Lab 2 Photo 1]({{ site.baseurl }}/images/path_lab2_photo_1.jpg) 
The previous method doesn’t work


![Path Traversal Lab 2 Photo 2]({{ site.baseurl }}/images/path_lab2_photo_2.jpg) 
We add another path traversal 
Because it is stripped off non recursively

---

## Lab 3 — Traversal sequences stripped with superfluous URL-decode

**What this teaches (one line):**
How double-encoding or extra URL decoding can bypass checks that don’t fully normalize input.

**Simple beginner walkthrough:**

1. Capture the image request and edit the filename parameter in Burp.
2. Use a double-encoded traversal value such as: `..%252f..%252f..%252fetc/passwd`.
3. Send the modified request; if `/etc/passwd` is returned, the server decoded the value and allowed traversal despite the filter.


![Path Traversal Lab 3 Photo 1]({{ site.baseurl }}/images/path_lab3_photo_1.jpg) 
The method used for the previous lab doesn’t work


![Path Traversal Lab 3 Photo 2]({{ site.baseurl }}/images/path_lab3_photo_2.jpg) 
Double url encode characters


---

## Lab 4 — File extension validation bypass with null byte

**What this teaches (one line):**
How appending a null byte can bypass simple checks that only look at the filename extension.

**Simple beginner walkthrough:**

1. Intercept the product image request with Burp Suite.
2. Change the filename parameter to include a null byte after the target name, for example: `../../../etc/passwd%00.png`.
3. Send the edited request. If the response contains `/etc/passwd`, the server likely accepted the request as a `.png` but actually opened the earlier filename because of the null byte — showing the extension check was bypassed.

![Path Traversal Lab 4 Photo 1]({{ site.baseurl }}/images/path_lab4_photo_1.jpg) 
Add null byte feature



