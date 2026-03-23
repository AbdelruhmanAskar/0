---
layout: single  
title: "CAT CTF 26: Entry Level Write-ups"  
date: 2026-03-23  
categories: \[Writeups, OSINT, Web, Reverse Engineering, Forensics, Crypto, Linux, Network\]  
tags: \[CTF, Cybersecurity\]  
author\_profile: true
---

🏆 CAT CTF 26: Entry Level Write-ups

What’s up, hackers! 👋

![Meme](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/meme.jpg)

I’m back with a massive write-up series for **CAT CTF 2026**. This competition was a wild ride, and I’m hyped to announce that I managed to secure **4th Place** overall! 🏆

It wasn't easy, but the grind was worth it. Here’s a quick flex of what happened during the event:

*   **First Blood 🩸:** Snagged the first solve on **2 OSINT** challenges.
    
*   **The Welcome Flag Sniper 🎯** 
    
*   **Web Exploitation 🕸️:** Secured **2nd Place** on one Web challenge and **3rd Place** on another.

I want to give a huge shout-out to all the authors for these amazing challenges. Whether it was **Reverse, Forensics, Web, Network, Linux, or Crypto**, the quality was top-notch and kept me on the edge of my seat.

Enough talking, let's get into the technical stuff. We’re going to cover everything, but we’ll kick things off with my favorite playground: **OSINT**.

---

🕵️‍♂️ OSINT Series: Who Will Win the Million?
==============================================

Today I’m sharing a special write-up for a OSINT challenge that was easy. I managed to snag the **First Blood (First Solve) 🩸**.

It was a trivia-style survival game where you had to answer 12 OSINT questions in a row. One typo, one wrong date, or one wrong format, and the connection drops. It's all about precision and fast "Google Dorking" skills.  

Below is the full walkthrough of how I hunted down the answers and secured the flag. 

![challenge](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/challenge.png)

**Author:** 0x2face

**Points:** 100

📝 The Challenge Description
---
 
> `nc 178.62.202.60 8080`

* * *

🔍 Phase 1: The Connection
--------------------------

Connecting to the server gives us a cool ASCII art intro. I chose option `1` to start the hunt.

![nc](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/nc.png)

* * *

🚇 Phase 2: The 12 Questions Walkthrough
----------------------------------------

### 1\. Microsoft Hafnium Attack

*   **Question:** What is the date Microsoft disclosed the exchange server hafnium attack?
    
*   **Format:** `DD-MM-YYYY`
    
*   **Search:** "Microsoft exchange server hafnium disclosure date"
    
*   **Answer:** `02-03-2021`
    
*   **Reference:** [Talos Intelligence](https://blog.talosintelligence.com/threat-advisory-hafnium-and-microsoft/)
    
![q10](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/q10.png)

* * *

### 2\. Yahoo Data Breach

*   **Question:** What is the date of the data breach of the 500m accounts of yahoo?
    
*   **Format:** `DD-MM-YYYY`
    
*   **Search:** "Yahoo 500 million accounts breach date"
    
*   **Answer:** `22-09-2016`
    
*   **Reference:** [Wikipedia](https://en.wikipedia.org/wiki/Yahoo_data_breaches)

![q7](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/q7.png)

* * *

### 3\. The Silk Road

*   **Question:** What is the real name of the founder of the silk road dark web marketplace?
    
*   **Format:** `Fname_Lname`
    
*   **Search:** "Silk Road marketplace founder"
    
*   **Answer:** `Ross_Ulbricht`
    
*   **Reference:** [Wikipedia](https://en.wikipedia.org/wiki/Ross_Ulbricht)

![q9](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/q9.png)

* * *

### 4\. LulzSec Leader "Sabu"

*   **Question:** What is the real name of the hacker known as “Sabu” who was a leader of lulzsec?
    
*   **Format:** `Fname_Lname`
    
*   **Search:** "Sabu hacker real name"
    
*   **Answer:** `Hector_Monsegur`
    
*   **Reference:** [Wikipedia](https://en.wikipedia.org/wiki/Hector_Monsegur)

  ![q3](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/q3.png)

* * *

### 5\. Breach Forums Takedown

*   **Question:** What is the real name of the person arrested in 2023 who was associated with owning and operating breach forums?
    
*   **Format:** `Fname_Lname`
    
*   **Search:** "Breach Forums owner arrest 2023"
    
*   **Answer:** `Conor_Fitzpatrick`
    
*   **Reference:** [Department of Justice](https://www.justice.gov/archives/opa/pr/justice-department-announces-arrest-founder-one-world-s-largest-hacker-forums-and-disruption)
    
  ![q8](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/q8.png)

* * *

### 6\. The Condor

*   **Question:** What is the real name of the individual known as “the condor” in early phone phreaking history?
    
*   **Format:** `Fname_Lname`
    
*   **Search:** "The Condor hacker real name"
    
*   **Answer:** `Kevin_Mitnick`
    
*   **Reference:** [LA Times](https://www.latimes.com/archives/la-xpm-1995-02-18-mn-33388-story.html)

  ![q5](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/q5.png)

* * *

### 7\. WikiLeaks Founder

*   **Question:** What is the name of the founder of wikileaks?
    
*   **Format:** `Fname_Lname`
    
*   **Search:** "Founder of WikiLeaks"
    
*   **Answer:** `Julian_Assange`
    
*   **Reference:** [Wikipedia](https://en.wikipedia.org/wiki/Julian_Assange)

  ![q11](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/q11.png)

* * *

### 8\. Facebook Access Tokens

*   **Question:** What is the date facebook announced the breach exposing 50m access tokens?
    
*   **Format:** `DD-MM-YYYY`
    
*   **Search:** "Facebook 50m access tokens breach announcement date"
    
*   **Answer:** `28-09-2018`
    
*   **Reference:** [BBC News](https://www.bbc.com/news/technology-45686890)

  ![q4](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/q4.png)

* * *

### 9\. The Morris Worm

*   **Question:** What is the name of the hacker who created the morris worm in 1988?
    
*   **Format:** `Fname_Lname`
    
*   **Search:** "Creator of Morris Worm 1988"
    
*   **Answer:** `Robert_Morris` (Logic 101: The Morris worm was made by Morris 😂)
    
*   **Reference:** [Okta Identity 101](https://www.okta.com/identity-101/morris-worm/)
    
  ![q12](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/q12.png)

* * *

### 10\. Onion Routing Protocol

*   **Question:** What is the name of the founder of the onion routing protocol?
    
*   **Format:** `Fname_Lname`
    
*   **Search:** "Inventor of onion routing"
    
*   **Answer:** `Paul_Syverson`
    
*   **Reference:** [Wikipedia](https://en.wikipedia.org/wiki/Paul_Syverson)

  ![q2](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/q2.png)

* * *

### 11\. eBay Data Breach

*   **Question:** What is the date ebay disclosed its 145m user data breach?
    
*   **Format:** `DD-MM-YYYY`
    
*   **Search:** "eBay 145m data breach date"
    
*   **Answer:** `21-05-2014`
    
*   **Reference:** [Washington Post](https://www.washingtonpost.com/news/the-switch/wp/2014/05/21/ebay-asks-145-million-users-to-change-passwords-after-data-breach/)

  ![q6](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/q6.png)

* * *

### 12\. LinkedIn Credential Breach

*   **Question:** What is the date linkedin confirmed the massive credential breach?
    
*   **Format:** `DD-MM-YYYY`
    
*   **Search:** "LinkedIn 2016 breach confirmation date"
    
*   **Answer:** `18-05-2016`
    
*   **Reference:** [LinkedIn Help Center](https://www.linkedin.com/help/linkedin/answer/a1338522/notice-of-data-breach-may-2016?lang=en)
    
  ![q13](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/q13.png)

* * *

🏁 Phase 3: Extraction
----------------------

After getting through the gauntlet of questions, the "Millionaire" prompt finally changed! I was given the option to finally claim the prize.

> _"Congrats! You finished our OSINT questions and deserve the flag :)"_

  ![q13](https://raw.githubusercontent.com/AbdelruhmanAskar/0/refs/heads/master/assets/images/Entry%20Cat%20CTF/Osint/Who%20Win%20The%20Miliion/gettheflag.png)

I selected `1- get the flag` and got the final payload.

**Flag:** `CATF{0S1NT_1S_C00L_1F_U_KN0W_H0W_T0_USE_Y0UR_SE3RCH_SK11LS_W3LL}`
