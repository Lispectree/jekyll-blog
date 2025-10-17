# High-level summary

WebSocket vulnerabilities arise when real-time messages between the browser and server are not properly validated or escaped. Because WebSockets send raw messages, an attacker or tester can tamper with those messages to inject HTML or scripts that run in a user's browser. These labs show how to inspect and manipulate WebSocket traffic safely in a lab environment.

## Lab 1 — Manipulating WebSocket messages to exploit vulnerabilities

**What this teaches:** How editing live WebSocket messages can inject HTML that runs in connected browsers.

**Simple beginner walkthrough:**

1. Open the app's Live chat and send a normal message so WebSocket traffic appears in Burp.
2. In Burp Proxy, view the WebSockets history and notice the message frames being sent.
3. Send a message containing a `<` character and observe the client HTML-encodes it before sending.
4. Turn on WebSocket message interception in Burp, send another message, and intercept the outgoing frame.
5. Edit the intercepted frame to include `<img src=1 onerror='alert(1)'>`, forward it, and observe the alert in your browser (and the support agent's browser).

*Images (placeholders — add 2–3 screenshots):*

* web_sockets_manipulate_messages_step1.png
* web_sockets_manipulate_messages_step2.png
* web_sockets_manipulate_messages_step3.png


## Lab 2 — Manipulating the WebSocket handshake to exploit protections

**What this teaches:** How changing headers during the WebSocket handshake can bypass IP-based bans and allow obfuscated payloads to reach the server.

**Simple beginner walkthrough:**

1. Open Live chat and send a message so WebSocket traffic appears in Burp. In the WebSockets history, right-click a message and Send to Repeater.
2. Edit a message to include `<img src=1 onerror='alert(1)'>` and resend — observe the connection is closed and your IP is blocked.
3. In the WebSocket handshake request, add the header `X-Forwarded-For: 1.1.1.1` to spoof your IP and reconnect.
4. Send an obfuscated payload such as `<img src=1 oNeRrOr=alert`1`>` and observe it is accepted.
5. Confirm the payload executes in the target browser to solve the lab.

*Images (placeholders — add 2–3 screenshots):*

* web_sockets_handshake_spoof_step1.png
* web_sockets_handshake_spoof_step2.png
* web_sockets_handshake_spoof_step3.png


