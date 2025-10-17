# High-level summary

OAuth (Open Authorization) lets sites delegate authentication to third-party providers. Misconfigured OAuth flows, missing CSRF protection, or trusting redirects can let an attacker log in as other users, link attacker-controlled social accounts to victims, or steal access tokens. These labs show common OAuth pitfalls and simple, safe ways to test them in a lab environment.

## Lab 1 — Authentication bypass via OAuth implicit flow

**What this teaches:** How a site that trusts OAuth-provided user info and access tokens can be tricked into logging you in as another user.

**Simple beginner walkthrough:**

1. Using Burp's browser, perform the OAuth login flow so the blog authenticates you via the OAuth provider.
2. In Proxy > HTTP history, find the POST /authenticate request that the blog uses after the OAuth provider returns; send it to Repeater.
3. In Repeater, change the email field to `carlos@carlos-montoya.net` (or the target email) and resend the POST — the site accepts it.
4. Use "Request in browser -> In original session" on that modified request to open the URL in your browser; you should be logged in as Carlos.
5. Confirm you can access Carlos’s account to solve the lab.

![image alt](https://github.com/Lispectree/web-sec/blob/18ec95278de2f0bbd269de560f06601be14d7afe/web-security-labs/labs/oauth-authentication/OATH%20LAB1%20PHOTO1.jpg)
Sign in to the account with the details provided


![image alt](https://github.com/Lispectree/web-sec/blob/1787b3553fa915d3244e76303578427c63ac46d0/web-security-labs/labs/oauth-authentication/OATH%20LAB1%20PHOTO2.jpg)
In the request of that authentication 
You can change the email to any email address of your choice

## Lab 2 — Forced OAuth profile linking

**What this teaches:** **How missing CSRF/state in account-linking flows lets an attacker attach their social profile to another user’s account.**

**Simple beginner walkthrough:**

1. Log in normally to the blog and use "Attach a social profile" to start the OAuth linking flow; watch the GET /auth?client_id=... request in Proxy.
2. Turn on interception and begin the attach flow again; capture the redirect back to `/oauth-linking?code=...` and copy the full URL, then drop the request so the code remains unused.
3. On the exploit server, host an iframe with the copied `/oauth-linking?code=STOLEN-CODE` URL as the `src` and deliver it to the victim.
4. When the victim loads the exploit, their browser will follow the iframe URL and attach your social account to the victim’s blog account.
5. Now choose "Log in with social media" on the blog — you get logged in as the victim (admin); go to the admin panel and delete Carlos to solve the lab.

![image alt](https://github.com/Lispectree/web-sec/blob/53f534469ec4445245321a9052151a6f2341840d/web-security-labs/labs/oauth-authentication/OATH%20LAB2%20PHOTO1.jpg)
Try and add a social media after logging in


![image alt](https://github.com/Lispectree/web-sec/blob/fdc0f68905424542174345120ab256322eebfe27/web-security-labs/labs/oauth-authentication/OATH%20LAB2%20PHOTO2.jpg)
A code is generated to link your social media


![image alt](https://github.com/Lispectree/web-sec/blob/6c20f78abd655d35a5f3fd5b026a2b3f2997c967/web-security-labs/labs/oauth-authentication/OATH%20LAB2%20PHOTO3.jpg)
Craft an exploit on the exploit server


## Lab 3 — Stealing OAuth access tokens via an open redirect

**What this teaches:** How combining a lenient redirect_uri and an open redirect can be used to trick users into leaking OAuth access tokens.

**Simple beginner walkthrough:**

1. In Burp, observe the blog calling the OAuth userinfo `/me` endpoint after auth; send the GET /me request to Repeater so you can use it later.
2. Find a valid OAuth authorization request (GET /auth?...&redirect_uri=...) and test that `redirect_uri` accepts path traversal (e.g., `.../oauth-callback/../post?postId=1`).
3. Find the open redirect used by the "Next post" feature and craft a malicious authorization URL that sets `redirect_uri` to the blog open-redirect which forwards to your exploit server.
4. Host a small script on the exploit server that reads `location.hash` and sends it to your server (or logs it). Deliver the crafted authorization URL to the victim so their browser returns the token in the fragment to your exploit page.
5. Retrieve the stolen access token from your exploit server logs, place it into the Authorization header for the `/me` request in Repeater, fetch the victim’s data (including their API key), and submit it to solve the lab.
 
   ![image alt](https://github.com/Lispectree/web-sec/blob/52d2c61ff60e8e33dda4dab46af9178985df8a12/web-security-labs/labs/oauth-authentication/OATH%20LAB3%20PHOTO1.jpg)
   The redirect url that is prone to path traversal


![image alt](https://github.com/Lispectree/web-sec/blob/5290a9e47769e51dc42e82314760aefcfbb07a69/web-security-labs/labs/oauth-authentication/OATH%20LAB3%20PHOTO2.jpg)
Deliver a script to the victim


![image alt](https://github.com/Lispectree/web-sec/blob/d14920352c38f9a71faa995e75ba17bf49797fa8/web-security-labs/labs/oauth-authentication/OATH%20LAB3%20PHOTO3.jpg)
The access token of the victim



