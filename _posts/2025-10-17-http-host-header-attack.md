# High-level summary

Host header attacks abuse how a web application or its middleware uses the HTTP `Host` (or related) headers. If an app trusts unvalidated Host values, attackers can poison links in emails, cause cache poisoning, bypass routing checks, or trigger server-side requests to attacker-controlled hosts. These labs show common Host-header flaws and safe ways to test them in a lab environment.

## Lab 1 — Basic password reset poisoning

*What this teaches:How changing the Host header used when generating password-reset links can send tokens to an attacker-controlled domain.

**Simple beginner walkthrough:**

1. Request a password reset for your own account and open the reset email on the exploit server; note the reset link contains a temp token.
2. In Burp Repeater, resend the POST /forgot-password request but set the Host header to an arbitrary value — observe the reset email contains that host in the link.
3. Set the Host header to your exploit server domain and change the username to `carlos`, then send the request. Check your exploit server logs for the `temp-forgot-password-token` value for Carlos.
4. Take your original reset email URL, replace its token with the one you captured, visit it, and set a new password for Carlos.
5. Log in as Carlos to confirm the account was reset and solve the lab.

![image alt](https://github.com/Lispectree/web-sec/blob/643abe3857ecffcf649ef624a396826d1e17a1c4/web-security-labs/labs/http-host-header-attack/HEADER%20ATTACK%20LAB1%20PHOTO1.jpg)
The lab instructions


![image alt](https://github.com/Lispectree/web-sec/blob/403bc6dbd94c096903d67545fb5c8cdbcdc66760/web-security-labs/labs/http-host-header-attack/HEADER%20ATTACK%20LAB1%20PHOTO2.jpg)
Use that account we want to get the reset token as the forgot password 
And input our exploit server on it


![image alt](https://github.com/Lispectree/web-sec/blob/4c83cb1aef30510695156806701e2fc2877ab93c/web-security-labs/labs/http-host-header-attack/HEADER%20ATTACK%20LAB1%20PHOTO3.jpg)
The reset token of Carlos as seen in the exploit server

---

## Lab 2 — Web cache poisoning via ambiguous requests

*What this teaches:* **How ambiguous or duplicate Host headers can be used to inject attacker-controlled resource URLs into cached pages.**

**Simple beginner walkthrough:**

1. In Burp's browser, open the home page and send the GET / request to Repeater. Add a cache-buster query (e.g., `?cb=123`).
2. Note the server validates the primary Host header but ignores a second Host header value — the ignored value is reflected into an absolute import URL for `/resources/js/tracking.js`.
3. On the exploit server, create `/resources/js/tracking.js` containing `alert(document.cookie)` and store it.
4. In Repeater add a second `Host` header with your exploit server domain and resend until the response shows your exploit URL and `X-Cache: hit` (cached). Open the cached URL in a browser to confirm the alert triggers.
5. Replay as needed to keep the cache poisoned until the victim visits the page and the lab is solved.

![image alt](https://github.com/Lispectree/web-sec/blob/52026ba5a656284c21ae5521fb24d1259699b252/web-security-labs/labs/http-host-header-attack/HEADER%20ATTACK%20LAB2%20PHOTO1.jpg)
The response contains a file gotten from the host header


![image alt](https://github.com/Lispectree/web-sec/blob/f41f55e7b5070a36e9cc375581e25fd40f5ffabf/web-security-labs/labs/http-host-header-attack/HEADER%20ATTACK%20LAB2%20PHOTO2.jpg)
Add another header in the request and notice that the file has been manipulated 
Add your exploit server 
Add a payload in the exploit server that gets user cookie
Save it in the cache


## Lab 3 — Routing-based SSRF (IP brute-force via Host header)

*What this teaches:* **How manipulating the Host header can make backend middleware issue requests to arbitrary hosts and how to discover internal services (routing-based SSRF).**

**Simple beginner walkthrough:**

1. Send a working GET / request to Repeater and replace the Host header with a Collaborator domain (use Burp Collaborator). Send and poll Collaborator to confirm the server made requests to that domain.
2. Send the GET / request to Intruder, deselect "Update Host header to match target", and set the Host header to `192.168.0.§0§` with a Numbers payload 0–255 for the final octet.
3. Start the attack and look for a response with status `302` that redirects to `/admin`. Send that successful request to Repeater and change its path to `/admin` to load the admin panel.
4. From the admin panel, note the delete user form's CSRF token and session cookie. Craft a request to `/admin/delete` including the CSRF token and `username=carlos`, convert it to POST if needed, and send it to delete Carlos.
5. Confirm Carlos is deleted to solve the lab.

   ![image alt](https://github.com/Lispectree/web-sec/blob/40291f1eb1f381c2ec269b0a4753aeecfdb9ab64/web-security-labs/labs/http-host-header-attack/HEADER%20ATTACK%20LAB3%20PHOTO1.jpg)
   Admin page is not found


      ![image alt](https://github.com/Lispectree/web-sec/blob/4026257e97a71f634150179f4bb7dc3befb5d283/web-security-labs/labs/http-host-header-attack/HEADER%20ATTACK%20LAB3%20PHOTO2.jpg)
   Use intruder to find the internal ip address


      ![image alt](https://github.com/Lispectree/web-sec/blob/d6cbd75f2c3b4c2a6d1e114d03d9348b0388cbbe/web-security-labs/labs/http-host-header-attack/HEADER%20ATTACK%20LAB3%20PHOTO3.jpg)
   The status code of 200 indicate it is the internal ip


      ![image alt](https://github.com/Lispectree/web-sec/blob/9eca6696c462890c360a5eaa39d09d56a96cd0f6/web-security-labs/labs/http-host-header-attack/HEADER%20ATTACK%20LAB3%20PHOTO4.jpg)
   Edit the host request to the internal ip and delete the user
   
## Lab 4 — Host validation bypass via connection state attack

*What this teaches:* **How reusing a single connection (keep-alive) with carefully ordered requests can bypass Host-based routing checks.**

**Simple beginner walkthrough:**

1. Send the GET / request to Repeater, then duplicate the tab and put both tabs into a new tab group.
2. On the first tab keep the path `/` and the correct Host header; on the second tab set the path `/admin` and Host `192.168.0.1` (or as the lab instructs).
3. Set the group send mode to "Send group in sequence (single connection)" and set `Connection: keep-alive`. Send the sequence.
4. Check responses — the second request may be processed as `/admin` by the backend. Use the returned admin panel to find the delete form and copy the CSRF token and session cookie.
5. Craft and send a POST to `/admin/delete` with the CSRF token and `username=carlos` while preserving the session cookie to delete Carlos and solve the lab.


