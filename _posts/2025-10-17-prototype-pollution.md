# High-level summary

Prototype pollution happens when attacker-controlled input can modify built-in JavaScript prototypes (for example `Object.prototype`). This can change application behavior globally in the browser and often leads to DOM XSS by turning otherwise safe code into insecure sinks. Think of it like adding a new rule to every object — one injected property can make many parts of the app behave unexpectedly.

## Lab 1 — Client-side prototype pollution via browser APIs (value property)

**What this teaches (one line):** How injecting properties into `Object.prototype` via query parameters can influence DOM-insertion code and render attacker-controlled `<script>` elements.**

**Simple beginner walkthrough:**

1. In the browser, append `/?__proto__[foo]=bar` to the site URL and load it to test for a pollution source.
2. Open DevTools → Console and type `Object.prototype` — confirm the `foo` property is present.
3. Inspect the site JavaScript (Sources) and find code that uses a `value` property when building a `<script>` element (e.g. a `defineProperty` call that creates a property without a `value`).
4. Inject a `value` property via the query string: `/?__proto__[value]=data:,alert(1);` and reload. The page should insert a `<script src="data:,alert(1);">` (or similar) and execute the `alert()`.
5. The alert confirms the prototype pollution source and gadget are exploitable — lab solved.

![image alt](https://github.com/Lispectree/web-sec/blob/ff19da61ddebe1794f8d28644e76a59733bf2bc1/web-security-labs/labs/prototype-pollution/PROTO%20LAB1%20PHOTO1.jpg)
The search url can be used to contaminate the prototype


![image alt](https://github.com/Lispectree/web-sec/blob/c5d459e90d3d79d10f366b51a9d81f3e3797091b/web-security-labs/labs/prototype-pollution/PROTO%20LAB1%20PHOTO2.jpg)


![image alt](https://github.com/Lispectree/web-sec/blob/36a28e57bb99126b8be6e4e0b4f4e90b9e9f27d6/web-security-labs/labs/prototype-pollution/PROTO%20LAB1%20PHOTO3.jpg)
Use a payload to get an alert


## Lab 2 — DOM XSS via client-side prototype pollution (transport_url property)

**What this teaches (one line):** How adding a property like `transport_url` to `Object.prototype` can influence code that dynamically appends remote scripts and cause XSS.**

**Simple beginner walkthrough:**

1. Find the pollution vector by visiting `/?__proto__[foo]=bar` and verifying `Object.prototype` in DevTools includes your injected property.
2. Inspect the site’s scripts (Sources) and locate `searchLogger.js` which will append a `<script>` using a `transport_url` property if present.
3. Inject `transport_url` via the query string: `/?__proto__[transport_url]=data:,alert(1);` and reload the page.
4. Confirm an injected `<script>` tag with `src="data:,alert(1);"` appears and that `alert(1)` runs — lab solved.

![image alt](https://github.com/Lispectree/web-sec/blob/c716e53f0daa2a6c5cefa0d4381bba3db943d18e/web-security-labs/labs/prototype-pollution/PROTO%20LAB2%20PHOTO1.jpg)
Make use of the DOM invader option


![image alt](https://github.com/Lispectree/web-sec/blob/8c0cee7c6c425d7ed6a60a12c689ae3b1dd75323/web-security-labs/labs/prototype-pollution/PROTO%20LAB2%20PHOTO2.jpg)
The proto sources


![image alt](https://github.com/Lispectree/web-sec/blob/e03fe994c2765a09e66551ac668fee474ab9f491/web-security-labs/labs/prototype-pollution/PROTO%20LAB2%20PHOTO3.jpg)
The script for the prototype pollution being showed by the DOM invader


## Lab 3 — Client-side prototype pollution in third-party libraries (DOM Invader hitCallback)

**What this teaches (one line):** How security tools (DOM Invader) can help find prototype pollution sources and gadgets that reach sinks like `setTimeout` via `hitCallback`.**

**Simple beginner walkthrough:**

1. Open the lab in Burp’s browser, enable DOM Invader and enable the prototype pollution option, then reload the page.
2. Let DOM Invader scan for gadgets. When the scan finishes, view the results in DevTools → DOM Invader and note the `hitCallback` gadget reaching `setTimeout()`.
3. Click **Exploit** in DOM Invader to auto-generate a PoC — it should call `alert(1)` demonstrating the gadget is usable.
4. To attack a victim, host a navigation exploit on the exploit server that points the victim to `#__proto__[hitCallback]=alert(document.cookie)` and deliver it. When the victim loads the site, the polluted `hitCallback` will execute and leak cookies — lab solved.

![image alt](https://github.com/Lispectree/web-sec/blob/343fdc2f4b4a916b983b31b6976cecc8c02ed04b/web-security-labs/labs/prototype-pollution/PROTO%20LAB3%20PHOTO1.jpg)
The value of the prototype that can be polluted


![image alt](https://github.com/Lispectree/web-sec/blob/9a2db861f912b6f538b88c731002581a4395cda9/web-security-labs/labs/prototype-pollution/PROTO%20LAB3%20PHOTO2.jpg)
Deliver the exploit to the victim through the exploit server



