# High-level summary

Cross-Site Request Forgery (CSRF) tricks a logged-in user's browser into making unwanted actions on a website. If a site accepts state-changing requests without strong checks tied to the user session, an attacker can cause a victim to change settings or perform actions without their consent. These labs show common CSRF patterns and how to test them safely in a lab.

## Lab 1 — CSRF where token validation depends on request method

**What this teaches:** How relying on the HTTP method instead of a proper token can let attackers bypass CSRF protections.

**Simple beginner walkthrough:**

1. Log in and change your email while capturing the POST request in Burp Proxy.
2. Send the POST /my-account/change-email request to Repeater and notice the csrf field is required.
3. Convert the request to a GET (use "Change request method") and send it — the CSRF check is bypassed.
4. Host an auto-submitting HTML form on the exploit server that performs the GET change-email request for a victim.
5. Deliver the exploit to the victim to solve the lab.


## Lab 2 — CSRF where token is not tied to user session

**What this teaches (one line):** Why CSRF tokens must be unique per user/session and not reusable across sessions.

**Simple beginner walkthrough:**

1. In Burp, log in and submit the update-email form; capture the request and copy the CSRF token value.
2. Drop the request, then log in as a different user in a private browser and send the update-email request to Repeater.
3. Replace the CSRF token in the second session with the token copied from the first session and send it — the request succeeds.
4. Create a PoC exploit that includes a fresh token (tokens are single-use) and host it on the exploit server.
5. Deliver the stored exploit to the victim to change their email and solve the lab.




## Lab 3 — SameSite Strict bypass via client-side redirect

**What this teaches (one line):** How client-side redirects can be used to bypass SameSite protections under certain flows.

**Simple beginner walkthrough:**

1. Log in and confirm your session cookie uses `SameSite=Strict` so it isn’t sent on cross-site POSTs.
2. Post a comment and note the confirmation page URL `/post/comment/confirmation?postId=x` which later redirects in-browser.
3. Test a crafted `postId` like `../my-account` to confirm the client-side redirect reaches your account page.
4. Build an exploit page that navigates the victim to the confirmation URL with a `postId` that triggers a GET equivalent of the change-email action (URL-encode `?` and `&` as needed).
5. Deliver the exploit; when the victim’s browser follows the client-side redirect, the email change executes and the lab is solved.



## Lab 4 — CSRF where Referer validation depends on header being present

**What this teaches (one line):** Why relying on the presence of the Referer header is not a robust CSRF defense.

**Simple beginner walkthrough:**

1. Log in and submit the Update email form, then capture the request in Burp Proxy.
2. Send the request to Repeater and observe that changing the Referer header to another domain causes the request to be rejected.
3. Remove the Referer header entirely and send the request again — it is accepted.
4. Create an exploit HTML page that includes `<meta name="referrer" content="no-referrer">` to suppress the Referer header and host it on the exploit server.
5. Deliver the exploit to the victim so their browser sends the request without a Referer header and solves the lab.



