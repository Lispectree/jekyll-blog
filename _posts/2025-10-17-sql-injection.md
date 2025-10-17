# SQL Injection — Simple Walkthroughs

**High-level summary of SQL Injection**

SQL Injection (SQLi) happens when a website trusts the exact words you type and uses them to ask its database for information. If the site doesn’t check or change that text first, someone can type special words that change what the database does — for example, log in without a password, show hidden information, or make the site pause. The four labs below each show a simple, beginner-friendly example.

---

## Lab 1 — Login bypass (SQL injection vulnerability allowing login bypass)

**What this teaches (one line):**
How changing the username in the login message can make the site accept the login without the correct password.

**Simple beginner walkthrough:**

1. Go to the site’s login page in your browser.
2. Use Burp Suite to "catch" the message your browser sends when you try to log in (this lets you pause and edit that message).
3. In the caught message, replace the username with: `administrator'--`.
4. Forward (send) the edited message. The site will log you in as the administrator even though you did not enter the real password.

**Images (placeholders — add 2–3 screenshots):**

* `images/sqli_login_bypass_step1.png`
* `images/sqli_login_bypass_step2.png`
* `images/sqli_login_bypass_step3.png`

**Quick note:** Think of Burp catching the message like pausing a letter before the post office delivers it — you can open the letter, change it, and then let it be sent.

---

## Lab 2 — Querying database type & version (UNION-based SQLi)

**What this teaches (one line):**
How to add simple text to a page’s results to check where text appears, and then show the database version.

**Simple beginner walkthrough:**

1. Open a product page that filters by category.
2. Use Burp Suite to capture the request that asks the site to show products for that category.
3. Edit the captured request so the category part includes a short test that adds two words (for example `abc` and `def`) using a UNION-style trick. This test shows if those words appear on the product page.
4. After you see the test words on the page, send a second edit that asks the database to show its version. The version string will appear on the page where the test words were shown.

**Images (placeholders — add 2–3 screenshots):**

* `images/sqli_dbversion_step1.png`
* `images/sqli_dbversion_step2.png`
* `images/sqli_dbversion_step3.png`

**Quick note:** You are not breaking anything — you are just asking the site to show extra text where it already shows results.

---

## Lab 3 — Finding a text column (UNION attack — locate visible column)

**What this teaches (one line):**
How to find which position in the page output can display text so you know where to place visible values.

**Simple beginner walkthrough:**

1. Capture the same product-category request using Burp.
2. First send a test with placeholders (`NULL,NULL,NULL`) to see how many pieces of data the page expects.
3. Then replace each placeholder, one at a time, with a visible word like `abcdef` and send the request again.
4. When the page shows `abcdef`, you know that column can display text and you can use that position to show other data.

**Images (placeholders — add 2–3 screenshots):**

* `images/sqli_findtextcol_step1.png`
* `images/sqli_findtextcol_step2.png`
* `images/sqli_findtextcol_step3.png`

**Quick note:** Try one change at a time — replace only one placeholder per request so you can see exactly which spot shows text.

---

## Lab 4 — Blind SQL injection with time delay (pg\_sleep)

**What this teaches (one line):**
How to prove the site runs your injected command even when it won’t show results, by measuring how long the page takes to respond.

**Simple beginner walkthrough:**

1. Open the shop front page and intercept the request that includes the `TrackingId` cookie.
2. Edit the cookie value so it asks the database to wait for 10 seconds before replying: `TrackingId=x'||pg_sleep(10)--`.
3. Send the edited request. If the page takes about 10 seconds longer to respond, that proves the injection worked even though no data was shown.

**Images (placeholders — add 2–3 screenshots):**

* `images/sqli_blind_timedelay_step1.png`
* `images/sqli_blind_timedelay_step2.png`
* `images/sqli_blind_timedelay_step3.png`

**Quick note:** Time delays only show that the site ran your command; they don’t return data. Use them when the site hides its database output.

---

