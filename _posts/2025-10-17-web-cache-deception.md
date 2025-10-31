---
layout: post
title: Web Cache Deception write-up
date: 2025-10-17
categories: web-security
tags: [web cache deception, pentesting]
---
# High-level summary

Web cache deception attacks trick a cache into storing private pages under URLs that look like static files. If the cache later serves those URLs to other users, sensitive data (like API keys) can leak. These labs show how differences in how the origin server and the cache treat paths can be abused — think of it like hiding a private letter in a public folder with a harmless filename.

## Lab 1 — Exploiting path mapping for web cache deception

**What this teaches:** How adding a static extension to a private URL can cause the response to be cached and later served to others.

**Simple beginner walkthrough:**

1. Log in with wiener:peter and notice your API key appears on your account page.
2. In Burp, send the GET /my-account request to Repeater and change the path to /my-account/abc.js, then send it.
3. Note the response headers (Cache-Control: max-age and X-Cache: miss). Re-send quickly and see X-Cache: hit.
4. On the exploit server, create a small page that redirects Carlos to the cached URL (use a unique path so his view becomes cached).
5. Visit the crafted URL for Carlos, then open that cached URL yourself and copy Carlos’s API key to solve the lab.

![Web Cache Deception Lab 1 Photo 1]({{ site.baseurl }}/images/web_dec_lab1_photo_1.jpg)
The login shows your api key


![Web Cache Deception Lab 1 Photo 2]({{ site.baseurl }}/images/web_dec_lab1_photo_2.jpg)
The target api key will save in the cache


![Web Cache Deception Lab 1 Photo 3]({{ site.baseurl }}/images/web_dec_lab1_photo_3.jpg)
The target api key


## Lab 2 — Exploiting path delimiter discrepancies for web cache deception

**What this teaches:** How different path delimiters can reveal what the origin server accepts and help craft cacheable URLs.

**Simple beginner walkthrough:**

1. Log in with wiener:peter and confirm your API key appears on your account page.
2. Send the GET /my-account request to Repeater and try paths like /my-account/abc and /my-accountabc to see 404s.
3. Use Intruder to try delimiter characters after /my-account and discover that ? works (returns 200 with your API key).
4. In Repeater, try adding an encoded dot-segment like /aaa/..%2fmy-account to confirm the origin resolves it to /my-account.
5. Craft a cached URL using the resources prefix (e.g. /resources/..%2fmy-account?wcd), deliver it to Carlos from the exploit server, then open it yourself and copy his API key.

![Web Cache Deception Lab 2 Photo 1]({{ site.baseurl }}/images/web_dec_lab2_photo_1.jpg)
This shows cache is being used


![Web Cache Deception Lab 2 Photo 2]({{ site.baseurl }}/images/web_dec_lab2_photo_2.jpg)
A delimeter that allows for the manipulation for the query is “?”


![Web Cache Deception Lab 2 Photo 3]({{ site.baseurl }}/images/web_dec_lab2_photo_3.jpg)
Deliver the exploit and access the api of the victim

## Lab 3 — Exploiting origin server normalization for web cache deception

**What this teaches:** How differences in normalization (dot-segments, encoding) between origin and cache can be abused to cache private responses.

**Simple beginner walkthrough:**

1. Log in with wiener:peter and find your API key on the account page.
2. In Repeater, request a path like /aaa/..%2fmy-account and confirm it returns your account data.
3. Find a cached folder (such as /resources) and confirm that requests like /resources/aaa get cached (X-Cache goes from miss to hit).
4. Combine these to build /resources/..%2fmy-account, send it to create a cache entry, then deliver a link to Carlos from the exploit server.
5. Open the delivered URL, copy Carlos’s API key, and submit it to solve the lab.

![Web Cache Deception Lab 3 Photo 1]({{ site.baseurl }}/images/web_dec_lab3_photo_1.jpg)
 Evidence of the use of cache


![Web Cache Deception Lab 3 Photo 2]({{ site.baseurl }}/images/web_dec_lab3_photo_2.jpg)
  Using path traversal we are able to make a my-account request get saved in the cache by decieving the web app that it’s a request for resources


![Web Cache Deception Lab 3 Photo 3]({{ site.baseurl }}/images/web_dec_lab3_photo_3.jpg)
  Deliver the exploit
 


---

