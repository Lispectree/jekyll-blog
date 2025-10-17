# High-level summary

GraphQL APIs are flexible and powerful, but misconfigurations or insufficient access controls can expose sensitive data or allow destructive actions. These labs explore three common GraphQL issues: accessing hidden data, discovering hidden endpoints, and bypassing brute-force protections.

---

## Lab 1 — Accessing private GraphQL posts

**What this teaches:** How sequential IDs and exposed fields in GraphQL can reveal hidden posts and sensitive data.

**Simple beginner walkthrough:**

1. Open the lab and visit the blog page in Burp's browser.
2. Intercept the GraphQL request fetching blog posts. Notice that post ID 3 is missing from the response — this is a hidden post.
3. Send the `/graphql/v1` POST request to Repeater.
4. In the GraphQL tab, add the `postPassword` field to the query for post ID 3.
5. Send the request and copy the `postPassword` from the response. Submit it to solve the lab.
![image alt](https://github.com/Lispectree/web-sec/blob/bb6883e86fb80e91631886e7aa6d4081b323c1aa/web-security-labs/labs/graphql-vulnerabilities/GRAPHQL%20LAB1%20PHOTO1.jpg)
After trying multiple graphql endpoints it showed 
“Method not allowed”
Instead of “not found”
Which is evidence of this endpoint existing


![image alt](https://github.com/Lispectree/web-sec/blob/2a7112fa7df3c4a152af8d00afbfa4c8c6701326/web-security-labs/labs/graphql-vulnerabilities/GRAPHQL%20LAB1%20PHOTO2.jpg)
Change the request to POST and add a json file as instructed 
Preferably to read the schema


![image alt](https://github.com/Lispectree/web-sec/blob/f42c348e194cc353e01be0e4dbaa15d635fbf69a/web-security-labs/labs/graphql-vulnerabilities/GRAPHQL%20LAB1%20PHOTO3.jpg)
Add a post password query that was discovered in the schema




---

## Lab 2 — Finding a hidden GraphQL endpoint

**What this teaches:** How hidden endpoints and introspection protections can be bypassed to discover schema details and manipulate data.

**Simple beginner walkthrough:**

1. Use Burp Repeater to send GET requests to potential endpoints like `/api` with the query parameter `/api?query=query{__typename}`.
2. Confirm that the response indicates a GraphQL endpoint.
3. Send a URL-encoded introspection query. If blocked, add a newline after `__schema` to bypass regex-based filtering.
4. Explore the schema to identify the `getUser` query and `deleteOrganizationUser` mutation.
5. Send a mutation with `id=3` to delete the user `carlos` and solve the lab.

![image alt](https://github.com/Lispectree/web-sec/blob/53b3f6c5f5be26f63dbb68b8cba6f39ae668f6e1/web-security-labs/labs/graphql-vulnerabilities/GRAPHQL%20LAB2%20PHOTO1.jpg)
This endpoint shows query not present 
Instead of not found as shown in others
Indicating this endpoint is a hidden graphql endpoint


![image alt](https://github.com/Lispectree/web-sec/blob/952208ee8fcd0e83a7c7fd27236c0f6598657589/web-security-labs/labs/graphql-vulnerabilities/GRAPHQL%20LAB2%20PHOTO2.jpg)
URL encode the request


![image alt](https://github.com/Lispectree/web-sec/blob/0d756628f35eb4cb3f7b169a89ca41df6fd1b25a/web-security-labs/labs/graphql-vulnerabilities/GRAPHQL%20LAB2%20PHOTO3.jpg)
Delete Carlos user
And complete the lab


## Lab 3 — Bypassing GraphQL brute force protections

**What this teaches:** How aliases can be used to circumvent rate limits and attempt multiple login mutations in a single request.

**Simple beginner walkthrough:**

1. Intercept the login GraphQL mutation in Burp.
2. In Repeater, craft a single mutation containing multiple aliases. Each alias attempts a different password for the user `carlos`.
3. Remove the `operationName` and variable dictionary fields if present.
4. Send the aliased mutation request.
5. Inspect the response to find the alias with `success: true`.
6. Log in with the discovered password to solve the lab.

![image alt](https://github.com/Lispectree/web-sec/blob/6cd412bfe48abfac51febd64e86fe6e1811eacad/web-security-labs/labs/graphql-vulnerabilities/GRAPHQL%20LAB3%20PHOTO1.jpg)
Shows that a brute force protection is present


![image alt](https://github.com/Lispectree/web-sec/blob/a1faa3bad24cc9b9c5ef50e84e12ef63858aeccd/web-security-labs/labs/graphql-vulnerabilities/GRAPHQL%20LAB3%20PHOTO2.jpg)
After using aliases 
We are able to access the login details using brute force



