---
layout: post
title: OS Command Injection write-up
date: 2025-10-17
categories: web-security
tags: [os command injection, pentesting]
---
# High-level summary
OS Command Injection happens when a website takes text you give it and runs that text as a command on the server. If the site doesn’t check or limit that text, someone can add extra commands to be executed — for example, to find the username the server runs as, or to write command output to a file and read it later. This is dangerous because it lets an attacker run commands on the server itself.

---

## Lab 1 — OS command injection (simple case)

**What this teaches:**
How adding a command separator and a small system command to an input can make the server run that command and return its output.

**Simple beginner walkthrough:**

1. Open the page that checks stock and use Burp Suite to intercept the request it sends.
2. Edit the `storeID` value so it contains a command, for example: `1|whoami`. The `|` means “run this command too.”
3. Send the edited request and look at the response — if it contains a username (the result of `whoami`), the server executed your command.
![OS Command Lab 1 Photo 1]({{ site.baseurl }}/images/os_lab1_photo_1.jpg)
Lab instructions


![OAUTH Lab 1 Photo 2]({{ site.baseurl }}/images/oath_lab1_photo_2.jpg)
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

![OAUTH Lab 2 Photo 1]({{ site.baseurl }}/images/oath_lab2_photo_1.jpg)
The email section is vulnerable to command injection


![OAUTH Lab 2 Photo 2]({{ site.baseurl }}/images/oath_lab2_photo_2.jpg)
But it doesn’t disclose anything 
So we test it by telling the server to sleep for 5sec before responding


![OAUTH Lab 2 Photo 3]({{ site.baseurl }}/images/oath_lab2_photo_3.jpg)
We that use redirection to disclose the whoami command inside a txt file


---

