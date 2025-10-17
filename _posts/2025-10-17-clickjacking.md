# High-level summary

Clickjacking tricks a user into clicking something on a trusted site by hiding that site in a transparent frame and placing attractive buttons over it. It’s like placing a transparent overlay over a real button and getting someone to click the overlay instead. These labs show simple ways attackers can build clickjacking pages and why sites should use frame-busting or frame-allow policies.

## Lab 1 — Basic clickjacking with CSRF token protection

**What this teaches:** How a transparent iframe and a decoy button can trick a user into performing a protected action.

**Simple beginner walkthrough:**

1. Log in to your account on the target site.
2. On the exploit server, paste the provided HTML template and set the iframe `src` to your lab's `/my-account` page.
3. Adjust the width, height, top, left and opacity values so the decoy text covers the real "Delete account" button; test with low opacity first.
4. Change the decoy text to "Click me", store the exploit, then use "Deliver exploit to victim".
5. When the victim clicks the decoy, the hidden iframe triggers the account deletion and the lab is solved.

![image alt](https://github.com/Lispectree/web-sec/blob/54b1c23748d32499e6f705f15fef1c76cc3c22b6/web-security-labs/labs/clickjacking/CLICKJACKING%20LAB1%20PHOTO1.jpg)
The payload which we will put the underneath the account page


## Lab 2 — Clickjacking with a frame buster script

**What this teaches:** How using sandboxing and careful framing can bypass simple frame-buster scripts.

**Simple beginner walkthrough:**

1. Log in and open the exploit server editor.
2. Use the supplied HTML template; add `sandbox="allow-forms"` to the iframe and set the `src` to your `/my-account?email=...` URL.
3. Adjust the iframe size and position values so the decoy aligns with the "Update email" form button; use low opacity while aligning.
4. Change the decoy text to "Click me", store the exploit and deliver it to the victim.
5. When the victim clicks, the hidden form submits and the lab is solved.


![image alt](https://github.com/Lispectree/web-sec/blob/54a2535e93107cfb444d1024193568e2d23b0a4c/web-security-labs/labs/clickjacking/CLICKJACKING%20LAB2%20PHOTO1.jpg)



## Lab 3 — Multistep clickjacking

**What this teaches:** How chaining clicks (two-step) can confirm an action that requires a confirmation page.

**Simple beginner walkthrough:**

1. Log in and open the exploit server editor.
2. Paste the multistep HTML template and set the iframe `src` to your lab's `/my-account` page.
3. Set the positions for the two decoy elements so the first click hits "Delete account" and the second hits the confirmation "Yes" button.
4. Test alignment with low opacity, then change the decoy text to "Click me first" and "Click me next", store the exploit, and deliver it to the victim.
5. When the victim clicks both decoys in order, the account is deleted and the lab is solved.


