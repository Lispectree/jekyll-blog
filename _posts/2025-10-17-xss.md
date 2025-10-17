# High-level summary

Cross-site scripting (XSS) happens when a web app displays attacker-controlled text as active content (like HTML or JavaScript). Attackers can use XSS to run scripts in other users’ browsers, steal cookies, or perform actions as those users. These labs show common XSS types and simple ways to test them safely in a lab environment.

## Lab 1 — Stored XSS into HTML context with nothing encoded

**What this teaches:** How saving unencoded input can make a comment box run JavaScript for anyone who reads it.

**Simple beginner walkthrough:**

1. Go to a blog post with a comment box.
2. Type `<script>alert(1)</script>` into the comment field and fill in the name, email, and website.
3. Click "Post comment" and then return to the blog page.
4. Observe the alert popup (the script ran) — that shows stored XSS.

*Images (placeholders — add 2–3 screenshots):*

* xss_stored-xss-html_step1.png
* xss_stored-xss-html_step2.png
* xss_stored-xss-html_step3.png

*Quick note:* Only test stored XSS in lab environments; don’t post scripts on real websites.

## Lab 2 — DOM XSS in jQuery anchor href attribute sink using location.search source

**What this teaches:** How putting untrusted text into a link (href) can run code when the link is used.

**Simple beginner walkthrough:**

1. On the Submit feedback page, change the `returnPath` query parameter to `/` plus a short random string and confirm it appears in the page.
2. Edit `returnPath` to `javascript:alert(document.cookie)` in the address bar and press Enter.
3. Click the page’s "back" link (which uses that `returnPath`) and observe the alert running.
4. That shows a DOM XSS via an unsafe href value.

*Images (placeholders — add 2–3 screenshots):*

* xss_dom-jquery-href_step1.png
* xss_dom-jquery-href_step2.png
* xss_dom-jquery-href_step3.png

*Quick note:* Only try DOM XSS tests in safe lab environments; manipulating links can have unexpected effects.

## Lab 3 — Reflected DOM XSS via JSON eval and backslash handling

**What this teaches:** How poorly escaped JSON used with `eval()` can be turned into executable script.

**Simple beginner walkthrough:**

1. Use the site search and send the search request through Burp so you can edit it.
2. Observe the JSON `search-results` response is later used by code that calls `eval()`.
3. Search for the string `\"-alert(1)}//` (enter it exactly as shown) and submit.
4. The response is crafted so the JSON becomes executable and the alert runs — showing reflected DOM XSS.

*Images (placeholders — add 2–3 screenshots):*

* xss_reflected-dom-json_step1.png
* xss_reflected-dom-json_step2.png
* xss_reflected-dom-json_step3.png.

## Lab 4 — Reflected XSS with some SVG markup allowed

**What this teaches:** How filtering that blocks common tags can still allow XSS through special SVG tags and event attributes.

**Simple beginner walkthrough:**

1. Try a simple payload like `<img src=1 onerror=alert(1)>` in the search — it’s blocked.
2. Send the search request to Burp Intruder and place a payload position inside `<>`.
3. Use the XSS cheat sheet tags list as payloads and run the attack to find tags that are not blocked (e.g., `<svg>` and `<animatetransform>`).
4. Test event attributes from the cheat sheet and find `onbegin` works; build the final URL that uses `<svg><animatetransform onbegin=alert(1)>` and visit it to trigger the alert.

*Images (placeholders — add 2–3 screenshots):*

* xss_reflected-svg_allowed_step1.png
* xss_reflected-svg_allowed_step2.png
* xss_reflected-svg_allowed_step3.png

