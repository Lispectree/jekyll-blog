# High-level summary

Server-Side Request Forgery (SSRF) occurs when an application allows users to make requests from the server to internal or external systems. Attackers can exploit SSRF to access restricted resources, internal networks, or perform actions on behalf of the server. Learning about SSRF helps developers validate and sanitize user-provided URLs to prevent unauthorized access.

## Lab 1 — Basic SSRF Against the Local Server

**What this teaches:** How SSRF can let you access local server pages.

**Simple beginner walkthrough:**

1. Try to browse to /admin directly — notice it’s blocked.
2. Visit a product, click "Check stock," intercept the request, and send it to Repeater.
3. Change the stockApi URL to [http://localhost/admin](http://localhost/admin) and forward it.
4. Read the HTML to find the delete URL and submit it in the stockApi parameter.

![image alt](https://github.com/Lispectree/web-sec/blob/baca2f7309a17d0011fc5d94d2821703798a898e/web-security-labs/labs/ssrf/SSRF%20LAB1%20PHOTO1.jpg)
The stock api
Discloses that there’s a url that can be manipulated by the client used to generate stock check


![image alt](https://github.com/Lispectree/web-sec/blob/b70e6e0a844672d43cdfde4e707a7f15d3c11c45/web-security-labs/labs/ssrf/SSRF%20LAB1%20PHOTO2.jpg)
Change the url to local host and see that you can access the admin panel

 **Lab 2** 
**What this teaches:** How SSRF can target other internal servers by guessing IP addresses.

**Simple beginner walkthrough:**

1. Click "Check stock" on a product and intercept the request.
2. Send it to Burp Intruder, change the stockApi URL to [http://192.168.0.1:8080/admin](http://192.168.0.1:8080/admin).
3. Highlight the last octet, add §, set payload type to Numbers, From 1, To 255, Step 1, and start the attack.
4. Identify the request returning status 200, send it to Repeater, and change the path to delete carlos.

![image alt](https://github.com/Lispectree/web-sec/blob/bb2f577387931255d997222847f3dc147a35fdf3/web-security-labs/labs/ssrf/SSRF%20LAB2%20PHOTO1.jpg)
A url that can be manipulated by the user


![image alt](https://github.com/Lispectree/web-sec/blob/e055d0dc887fc4b8261726627756cce6f6d3f682/web-security-labs/labs/ssrf/SSRF%20LAB2%20PHOTO2.jpg)
Using intruder try and get the ip of the local host


![image alt](https://github.com/Lispectree/web-sec/blob/2fa920f648897bdc3a029450a677900f7022938f/web-security-labs/labs/ssrf/SSRF%20LAB2%20PHOTO3.jpg)
Access the admin apge

## Lab 3 — SSRF with Blacklist-Based Input Filter

**What this teaches :** How SSRF input filters can be bypassed using URL obfuscation.

**Simple beginner walkthrough:**

1. Click "Check stock" and intercept the request, send to Repeater.
2. Change the stockApi URL to [http://127.0.0.1/](http://127.0.0.1/) — notice it’s blocked.
3. Use [http://127.1/](http://127.1/) instead, then [http://127.1/admin](http://127.1/admin) — blocked again.
4. Double URL encode the "a" to %2561 to access the admin interface and delete carlos.

![image alt](https://github.com/Lispectree/web-sec/blob/9546dbce6fc588d4373a53f28d5ae4cfb43098f8/web-security-labs/labs/ssrf/SSRF%20LAB3%20PHOTO1.jpg)


![image alt](https://github.com/Lispectree/web-sec/blob/b85ae9dcb80c910ebd590a2ecedcbbefc8094d5a/web-security-labs/labs/ssrf/SSRF%20LAB3%20PHOTO2.jpg)
Local host isn’t as the local host ip is blacklisted


![image alt](https://github.com/Lispectree/web-sec/blob/9b514cafe21778a4c694104f1d417ba576970557/web-security-labs/labs/ssrf/SSRF%20LAB3%20PHOTO3.jpg)
Removing two octet values of the blacklisted local host ip
Makes it accessible




## Lab 4 — SSRF via Open Redirection Vulnerability

**What this teaches:** How an open redirection can be used to bypass SSRF restrictions.

**Simple beginner walkthrough:**

1. Click "Check stock," intercept the request, and send to Repeater.
2. Notice you can’t make requests to a different host directly.
3. Exploit the open redirection using the nextProduct path: /product/nextProduct?path=[http://192.168.0.12:8080/admin](http://192.168.0.12:8080/admin).
4. Amend the path to delete carlos: /product/nextProduct?path=[http://192.168.0.12:8080/admin/delete?username=carlos](http://192.168.0.12:8080/admin/delete?username=carlos) and forward the request.

![image alt](https://github.com/Lispectree/web-sec/blob/f1b89932c52f2d1b43304d91a1a05bc525952839/web-security-labs/labs/ssrf/SSRF%20LAB4%20PHOTO1.jpg)


![image alt](https://github.com/Lispectree/web-sec/blob/0aaedd8da1cf6396365786ff3ea861d448bea7b8/web-security-labs/labs/ssrf/SSRF%20LAB4%20PHOTO2.jpg)
Local host can’t be accessed through stock api


![image alt](https://github.com/Lispectree/web-sec/blob/316e3d71e274bdb4b0c00318cc7ebcfcd2881493/web-security-labs/labs/ssrf/SSRF%20LAB4%20PHOTO3.jpg)
Use the url that redirects to access the admin panel

