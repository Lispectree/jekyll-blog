---
layout: post
title: "Insecure Deserialization write-up"
date: 2025-10-17
categories: web-security
tags: [insecure deserialization, pentesting]
---
# High-level summary

Insecure deserialization happens when an application accepts serialized data (structured data turned into a string) from a user and restores it without checking it first. Attackers can modify that serialized data to change program logic — for example, flipping an "admin" flag — which can give them extra privileges or cause unexpected behavior. These labs show simple, hands-on examples of that risk.

## Lab 1 — Modifying serialized objects

**What this teaches:** How changing values inside a serialized object (like a PHP session) can escalate privileges.

**Simple beginner walkthrough:**

1. Log in and find the request that includes your session cookie in Burp's Proxy history; note the cookie looks encoded.
2. Use Burp's Inspector to decode the cookie and see it's a serialized PHP object; send the request to Repeater.
3. In Repeater's Inspector, change the `admin` value from `b:0` to `b:1`, apply changes so Burp re-encodes the cookie, then resend the request.
4. Visit `/admin` and perform the admin action (for example, `/admin/delete?username=carlos`) to solve the lab.

![Insecure Deserialization Lab 1 Photo 1]({{ site.baseurl }}/images/insecure_deserialization_lab1_photo_1.jpg)
Lab instructions


![Insecure Deserialization Lab 1 Photo 2]({{ site.baseurl }}/images/insecure_deserialization_lab1_photo_2.jpg)
The decoded session cookie contains parameters that are predictable


![Insecure Deserialization Lab 1 Photo 3]({{ site.baseurl }}/images/insecure_deserialization_lab1_photo_3.jpg)
Change that admin to true and edit the session cookie

## Lab 2 — Arbitrary object injection in PHP

**What this teaches:** How crafting a serialized object of a known class can trigger dangerous behavior (like deleting files) when the object is deserialized.

**Simple beginner walkthrough:**

1. Log in and confirm your session cookie contains a serialized PHP object.
2. In the site map, find `/libs/CustomTemplate.php`, send it to Repeater, and append a `~` to read the source code; look for a `__destruct()` method that calls `unlink()` on a `lock_file_path` attribute.
3. In Burp Decoder, craft a serialized `CustomTemplate` object with `lock_file_path` set to `/home/carlos/morale.txt`, then Base64- and URL-encode it.
4. Replace your session cookie with the modified encoded object in Repeater and resend the request.
5. The server will deserialize the object and the destructor will delete Carlos's file — confirm the file is gone to solve the lab.

![Insecure Deserialization Lab 2 Photo 1]({{ site.baseurl }}/images/insecure_deserialization_lab2_photo_1.jpg)
Account login page discloses presence of a php file


![Insecure Deserialization Lab 2 Photo 2]({{ site.baseurl }}/images/insecure_deserialization_lab2_photo_2.jpg)
The script of the file shows how a file can be deleted


![Insecure Deserialization Lab 2 Photo 3]({{ site.baseurl }}/images/insecure_deserialization_lab2_photo_3.jpg)
Draft a session token and delete morale.txt file

## Lab 3 — Exploiting Ruby deserialization using a documented gadget chain

**What this teaches:** How to reuse a public Ruby gadget chain to run commands when a Ruby object is deserialized.

**Simple beginner walkthrough:**

1. Log in and find the session cookie that contains a serialized Ruby object; send a request with this cookie to Burp Repeater.
2. Find the Universal Deserialisation Gadget for Ruby (vakzz) and copy the payload-generation script. Modify it to run `rm /home/carlos/morale.txt` instead of `id`, and change the final lines to `puts Base64.encode64(payload)`.
3. Run the modified script locally and copy the Base64-encoded output it produces.
4. In Burp Repeater, replace your session cookie with the Base64 payload (URL-encode it), then resend the request.
5. Confirm Carlos's file has been deleted to solve the lab.
 ![Insecure Deserialization Lab 3 Photo 1]({{ site.baseurl }}/images/insecure_deserialization_lab3_photo_1.jpg)
   Lab instructions


  ![Insecure Deserialization Lab 3 Photo 2]({{ site.baseurl }}/images/insecure_deserialization_lab3_photo_2.jpg)
   The decoded cookie shows username


     ![Insecure Deserialization Lab 3 Photo 3]({{ site.baseurl }}/images/insecure_deserialization_lab3_photo_3.jpg)
   Exploit


