# High-level summary

Race conditions occur when an application does not properly handle multiple operations happening at the same time. Attackers can exploit timing gaps to bypass restrictions, duplicate actions, or manipulate sensitive processes. Understanding these flaws helps developers design systems that safely manage simultaneous actions.

## Lab 1 — Limit Overrun Race Conditions

**What this teaches:** How sending multiple requests at the same time can bypass quantity or usage limits.

**Simple beginner walkthrough:**

1. Log in and add a cheap item to your cart using a discount code.
2. Identify the endpoints for adding items and applying discount codes.
3. In Burp Repeater, duplicate the POST /cart/coupon request into a group with 20 tabs.
4. Send the requests in parallel and observe that the discount is applied multiple times.
5. Add the leather jacket and repeat to reduce the total below your store credit, then complete the purchase.

![image alt](https://github.com/Lispectree/web-sec/blob/f41a5f6289c1781a414abd7e26e9b2368f7b5128/web-security-labs/labs/race-conditions/RACE%20LAB1%20PHOTO1.jpg)
After logging in it shows you have a promo code


![image alt](https://github.com/Lispectree/web-sec/blob/8be78f30fc21ce00b15040b16cb9dc61b698297b/web-security-labs/labs/race-conditions/RACE%20LAB1%20PHOTO2.jpg)
Try and acquire a product well above your balance by trying to make use of the promo code multiple times simultaneously through race conditions


![image alt](https://github.com/Lispectree/web-sec/blob/6c0f37010d904f7da6a1f911991d56dc6e912234/web-security-labs/labs/race-conditions/RACE%20LAB1%20PHOTO3.jpg)
Send all the request in parallel

## Lab 2 — Multi-endpoint Race Conditions

**What this teaches:** How exploiting timing across multiple endpoints can bypass purchase restrictions.

**Simple beginner walkthrough:**

1. Log in and add a gift card to your cart.
2. Identify the POST /cart and POST /cart/checkout endpoints.
3. In Burp Repeater, send the requests in parallel instead of sequentially.
4. Observe if the checkout succeeds even with insufficient funds.
5. Repeat with the leather jacket until the purchase completes successfully.
   ![image alt](https://github.com/Lispectree/web-sec/blob/258f8393cd40af5858257fbedacc312f3539af89/web-security-labs/labs/race-conditions/RACE%20LAB2%20PHOTO1.jpg)
   The lab


 ![image alt](https://github.com/Lispectree/web-sec/blob/7f9b0076a3434e24c992fdb961b7adfbf9039743/web-security-labs/labs/race-conditions/RACE%20LAB2%20PHOTO2.jpg)
 Send the cart and check out request simultaneously


  ![image alt](https://github.com/Lispectree/web-sec/blob/f3b74ec9f36f10c8625a7cf124ce8d44c23d1641/web-security-labs/labs/race-conditions/RACE%20LAB2%20PHOTO3.jpg)
  


## Lab 3 — Exploiting Time-Sensitive Vulnerabilities

**What this teaches:** How simultaneous requests can produce identical tokens for password resets.

**Simple beginner walkthrough:**

1. Submit a password reset for your account and observe the token in the email.
2. Send multiple POST /forgot-password requests in parallel using different session cookies.
3. Observe when two requests generate identical tokens.
4. Change the username in one request to target another user, like carlos.
5. Use the shared token to reset the other user's password and complete the lab.

  ![image alt](https://github.com/Lispectree/web-sec/blob/411126e5fc1e698baf4d3a1d6fcd920c9729c912/web-security-labs/labs/race-conditions/RACE%20LAB3%20PHOTO1.jpg)
  If you request for the forgot password functionality of two account 
It generates same token
Use that to access Carlos account

