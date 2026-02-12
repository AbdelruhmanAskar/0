---
layout: single
title: "N!ghtM4re CTF 2026: Web Challenges Writeups"
date: 2026-02-12
categories: [Writeups, WEB]
tags: [CTF, WEB , Author]
author_profile: true
---

# 🕸️ Web Writeups

Hello everyone!

Hello everyone! I’m really excited to share this post with you all. This time, things are a bit different! For the first time ever, I’ve stepped into the shoes of a Web Challenge Author for N!ghtM4re CTF 2026 🥳🥳.

It’s been an incredible experience moving from the "solver" side to the "architect" side. Designing these challenges was a lot of fun—thinking about how to hide the flags and creating tricky paths for the players really opened my eyes to how the web works from the inside out. I'm honestly so happy with how they turned out and seeing everyone's creative solutions was the best part of the journey.

In this post, I’ll be breaking down web challenges, ranging from Basic to Hard. Whether you're a beginner or a pro, I hope you find these writeups helpful and enjoy the logic behind them!

---

## 🚩 1. Challenge Write-up: Bl1nd Fa1th
=========================================

### 🛠️ General Information

*   **Challenge Name:** Bl1nd Fa1th
    
*   **Difficulty:** Basic
    
*   **Points:** 50
    
*   **Author:** D3xter
    
* * *

### 📜 The Description

> _"There is a rule in place. It was never written. It was never questioned."_

![Challenge Photo](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Bl1nd%20Fa1th/Challenge.png)

* * *

### 🔍 Stage 1: The Hidden Whisper (Reconnaissance)

Upon landing on the challenge page, I was greeted by a standard **Login** interface. No obvious hints, no flashy clues.

![Login Page](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Bl1nd%20Fa1th/Login.png)

But a true researcher knows that the best secrets are often hidden in plain sight.

I performed an **`Inspect Element`** to dive into the source code. Nestled within the comments, I found a developer's note left behind.

![Inspect](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Bl1nd%20Fa1th/Inspect.png)

The first piece of the puzzle:

*   **Target Username:** `@dmindex021`

* * *

### 🔓 Stage 2: Breaking the Logic (Exploitation)

Now I had the identity, but the password was still a black box. Instead of guessing or brute-forcing,

I decided to attack the underlying logic of the database.

I opted for a classic **SQL Injection (SQLi)** bypass. By injecting a logical tautology into the password field,

I could trick the server into validating the login regardless of the actual password.

*   **Input Payload:** `1' or 1=1--`
    

**The Breakdown:**

*   The `'` closes the original string.
    
*   The `or 1=1` creates a condition that is always **True**.
    
*   The `--` (comment) tells the database to ignore the rest of the original query.
    

* * *

### 🏆 Stage 3: System Access (The Flag)

The moment I hit **Login**, the system's defenses crumbled. The terminal screen flickered to life, granting me full access:

![Flag](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Bl1nd%20Fa1th/flag.png)

Plaintext

    AUTHENTICATED_SESSION
    ACCESS GRANTED
    [ ACTIVE_TERMINAL_SESSION ]
    > FLAG: N!ghtM4re{5ql_15_n0t_4_8u6_1t5_4_f34tur3!!}
    SESSION_STATE: STABLE 

**The Flag:** `N!ghtM4re{5ql_15_n0t_4_8u6_1t5_4_f34tur3!!}`
