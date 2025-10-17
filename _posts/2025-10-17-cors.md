# High-level summary

CORS (Cross-Origin Resource Sharing) controls whether a web page from one origin (site) can make requests to another origin and read the responses. Misconfigured CORS that reflects origins or trusts special origins (like `null`) can let a malicious site read sensitive data from a victim’s session. These labs teach how origin reflection and trusting `null` can be abused in a safe lab setting.

## Lab 1 — CORS vulnerability with basic origin reflection

**What this teaches:** How simply reflecting the `Origin` header lets an attacker read a victim’s cross-origin response.

**Simple beginner walkthrough:**

1. Log in using the lab browser and open your account page; find the AJAX request to `/accountDetails` in the proxy history.
2. Send that request to Repeater and add the header `Origin: https://example.com`, then resend; note that the response reflects the origin in `Access-Control-Allow-Origin`.
3. On the exploit server, paste the provided XMLHttpRequest PoC (replace YOUR-LAB-ID) so it requests `/accountDetails` with `withCredentials = true`.
4. View the exploit to run it locally and confirm the API key appears on the log page, then Deliver exploit to victim.
5. Open Access log, copy the victim’s API key and submit it to solve the lab.


![image alt](https://github.com/Lispectree/web-sec/blob/e96b6e212cf3cfe123659693eabcb3ff77857837/web-security-labs/labs/cors/CORS%20LAB1%20PHOTO1.jpg)
Account details response contains api key that is necessary in logging in


![image alt](https://github.com/Lispectree/web-sec/blob/ba847991d1994179bf781c71dd7195f3328bd6ed/web-security-labs/labs/cors/CORS%20LAB1%20PHOTO2.jpg)
Craft an exploit that gets the api key from the administrator upon clicking on the payload delivered


![image alt](https://github.com/Lispectree/web-sec/blob/2ef75d7070d20e615a94c27a2b7324274ac9db41/web-security-labs/labs/cors/CORS%20LAB1%20PHOTO3.jpg)
The api key of the administrator
## Lab 2 — CORS vulnerability with trusted null origin

**What this teaches:** How trusting the `null` origin (e.g., from sandboxed iframes) can let attackers read protected responses.

**Simple beginner walkthrough:**

1. Log in in the lab browser and find the `/accountDetails` AJAX request in Proxy history; confirm the server returns `Access-Control-Allow-Credentials`.
2. Send the request to Repeater and add the header `Origin: null`; resend and observe the server reflects `null` in `Access-Control-Allow-Origin`.
3. On the exploit server, create the provided sandboxed iframe PoC (replace YOUR-LAB-ID and YOUR-EXPLOIT-SERVER-ID) that issues a `withCredentials` request from a `null` origin.
4. View the exploit and confirm the API key ends up on the exploit server log page, then Deliver exploit to victim.
5. Retrieve the victim’s API key from the Access log and submit it to solve the lab.

![image alt](https://github.com/Lispectree/web-sec/blob/96456ab292d24d3e15a805f14e556381218cde98/web-security-labs/labs/cors/CORS%20LAB2%20PHOTO1.jpg)
Evidence of CORS usage


![image alt](https://github.com/Lispectree/web-sec/blob/b8870112878ab669ba18bf398d5a34bc93e532c0/web-security-labs/labs/cors/CORS%20LAB2%20PHOTO2.jpg)
Origin of null are accepted


![image alt](https://github.com/Lispectree/web-sec/blob/feaf54decd86f672cf12a216eaba6d8a68d83b81/web-security-labs/labs/cors/CORS%20LAB2%20PHOTO3.jpg)
Craft an exploit

