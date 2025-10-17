# Path Traversal — Simple Walkthroughs

**High-level summary of Path Traversal**

Path Traversal happens when a website lets you request files by name (for example, images). If the site doesn’t carefully check the filename you give it, you can ask for files outside the intended folder — including sensitive system files. Attackers use special filename text (like `../`) to move up the folder structure and read files the server shouldn’t reveal.

---

## Lab 1 — File path traversal (simple case)

**What this teaches :**
How a basic `../` sequence in a filename can let you read files outside the intended folder.

**Simple beginner walkthrough:**

1. Use Burp Suite to capture the request that fetches a product image.
2. Edit the filename parameter to: `../../../etc/passwd`.
3. Send the modified request and check the response — if it contains `/etc/passwd` contents, the server returned a file it shouldn't.
![image alt](https://github.com/Lispectree/web-sec/blob/df6bf18f5357fd39d9093d211e26a3d7e8be0187/web-security-labs/labs/path-traversal/PATH%20LAB1%20PHOTO1.jpg)
This request gets a jpg file from the server


![image alt](https://github.com/Lispectree/web-sec/blob/4acba78320b1f80718054a5c2f45bdfc250caa88/web-security-labs/labs/path-traversal/PATH%20LAB1%20PHOTO2.jpg)
Using path traversal we get the contents of etc/passwd


## Lab 2 — Traversal sequences stripped non-recursively

**What this teaches (one line):**
How naive filters that look for exact `../` sequences can be bypassed by slightly altered patterns.

**Simple beginner walkthrough:**

1. Capture the product image request in Burp.
2. Replace the filename value with: `....//....//....//etc/passwd`.
3. Send the request; if `/etc/passwd` is returned, the simple filter didn’t catch the altered traversal string.


![image alt](https://github.com/Lispectree/web-sec/blob/93a707b02d5cc482e6c809d3c083fa4c11a78bab/web-security-labs/labs/path-traversal/PATH%20LAB2%20PHOTO1.jpg)
The previous method doesn’t work


![image alt](https://github.com/Lispectree/web-sec/blob/e7e43c5188c827d97d0913f668cf03827b81432c/web-security-labs/labs/path-traversal/PATH%20LAB2%20PHOTO2.jpg)
We add another path traversal 
Because it is stripped off non recursively

---

## Lab 3 — Traversal sequences stripped with superfluous URL-decode

**What this teaches (one line):**
How double-encoding or extra URL decoding can bypass checks that don’t fully normalize input.

**Simple beginner walkthrough:**

1. Capture the image request and edit the filename parameter in Burp.
2. Use a double-encoded traversal value such as: `..%252f..%252f..%252fetc/passwd`.
3. Send the modified request; if `/etc/passwd` is returned, the server decoded the value and allowed traversal despite the filter.


![image alt](https://github.com/Lispectree/web-sec/blob/9e60008609c5ec9f34c06df6973cd870b8dfb035/web-security-labs/labs/path-traversal/PATH%20LAB3%20PHOTO1.jpg)
The method used for the previous lab doesn’t work


![image alt](https://github.com/Lispectree/web-sec/blob/e1c5abcbf9f50ff81c7f9af186c74accc5f0a739/web-security-labs/labs/path-traversal/PATH%20LAB3%20PHOTO2.jpg)
Double url encode characters


---

## Lab 4 — File extension validation bypass with null byte

**What this teaches (one line):**
How appending a null byte can bypass simple checks that only look at the filename extension.

**Simple beginner walkthrough:**

1. Intercept the product image request with Burp Suite.
2. Change the filename parameter to include a null byte after the target name, for example: `../../../etc/passwd%00.png`.
3. Send the edited request. If the response contains `/etc/passwd`, the server likely accepted the request as a `.png` but actually opened the earlier filename because of the null byte — showing the extension check was bypassed.

![image alt](https://github.com/Lispectree/web-sec/blob/f3e384d7ced143a6e49fc0554293041229800cfd/web-security-labs/labs/path-traversal/PATH%20LAB4%20PHOTO1.jpg)
Add null byte feature



