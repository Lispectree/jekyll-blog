# High-level summary

Business logic vulnerabilities happen when an application lets users bypass rules or manipulate normal processes. These flaws can let someone get extra items, discounts, or access areas they shouldn’t. Understanding these issues helps highlight the importance of proper checks in software.

## Lab 1 — Excessive trust in client side controls

**What this teaches:** How changing quantity values can trick the system into giving negative prices or extra items.

**Simple beginner walkthrough:**

1. Log in and add a cheap item to your shopping cart.
2. In Burp Suite, find the request that controls the item quantity.
3. Change the quantity to any number and forward the request. Check that the cart updates.
4. Try using a negative number to reduce the quantity. Use a bigger negative number to make the cart go below zero.
5. Add another item and use negative quantities to lower the total price below your store credit. Place the order to finish.

![image alt](https://github.com/Lispectree/web-sec/blob/d5479bd37ec967bc098faacbf2265f45dc886192/web-security-labs/labs/business-logic/BUS%20LOGIC%20photo1%20lab1.jpg)
After logging in 
You are required to buy the leather jacket
But your balance won’t be able to purchase it

![image alt](https://github.com/Lispectree/web-sec/blob/31ee40de032308b7897b4f345d1ed69b3b052709/web-security-labs/labs/business-logic/BUS%20LOGIC%20photo2%20lab1.jpg)
The add to cart request of the jacket
The price is client controlled and can be reduced to any price




## Lab 2 — Flawed enforcement of business rules

**What this teaches:** How improper checks let you reuse multiple discount codes to pay less.

**Simple beginner walkthrough:**

1. Log in and find the first coupon code (e.g., NEWCUST5).
2. Sign up for the newsletter to get another coupon (e.g., SIGNUP30).
3. Add an item to your cart and go to checkout.
4. Apply both codes, alternating between them to bypass the single-use rule.
5. Keep reusing the codes until the total is below your store credit, then place the order.

![image alt](https://github.com/Lispectree/web-sec/blob/6cd527c807c701226c9871142c7e48d1589805cc/web-security-labs/labs/business-logic/BUS%20LOGIC%20photo1%20lab2.jpg)
The first coupon

![image alt](https://github.com/Lispectree/web-sec/blob/a36fb4107e9cdae57a364df756aa041cfb921b22/web-security-labs/labs/business-logic/BUS%20LOGIC%20photo2%20lab2.jpg)
2nd coupon after signing up

![image alt](https://github.com/Lispectree/web-sec/blob/5daf4866636358cc01b2e7f5ed3db5cac7dbcb84/web-security-labs/labs/business-logic/BUS%20LOGIC%20photo3%20lab2.jpg)
Use the coupon alternatively to get a balance of $0 and purchase it





## Lab 3 — Authentication Bypass via Flawed State Machine

**What this teaches:** How skipping steps in login flow can give admin access.

**Simple beginner walkthrough:**

1. Log in and notice you select your role before reaching the home page.
2. Use a tool to find the /admin page. Trying to access it normally doesn’t work.
3. Log out, then log in again with Burp intercept on.
4. Forward the first login request, drop the next role-selection request, then go to the home page.
5. Observe you are now logged in as an administrator and complete the lab task.

![image alt](https://github.com/Lispectree/web-sec/blob/6f05485f85f1a2d676c031fed938e6c2b9683b83/web-security-labs/labs/business-logic/BUS%20LOGIC%20photo1%20lab3.jpg)
After logging in you are expected to select a role before you are granted access to

![image alt](https://github.com/Lispectree/web-sec/blob/fe3337010d2e7b42430f8674d053ac21dee83827/web-security-labs/labs/business-logic/BUS%20LOGIC%20photo2%20lab3.jpg)
Put on your intercept and drop the role selector request
This will give you access to the admin panel



