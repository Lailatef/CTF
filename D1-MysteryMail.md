D1-MysteryMail/writeup.md

# D1 — Mystery Mail 

**Platform:** CTF / Forensics  
**Category:** Email Forensics  
**Difficulty:** Easy

---

## 🎯 Goal
Identify the sender's original IP address from the provided email headers.

---

## 🧠 Approach
Email `Received:` headers are added by each mail server the message traverses. The earliest hop (the origin) is typically the bottom-most `Received:` line, so read the headers **bottom → top** to find the originating IP.

---

## 🛠️ Steps / Evidence
Headers provided (abridged):


Received: from 251.14.1.16 by mx3.personalyz.io; Sun, 23 Mar 2025 10:10:30 +0900
Received: from 250.24.46.164 by klaviyo.com; Sun, 23 Mar 2025 10:10:15 +0900
Received: from 252.44.98.29 by gwagm.co; Sun, 23 Mar 2025 10:09:50 +0900


Read bottom → top:
1. `252.44.98.29` → gwagm.co
2. `250.24.46.164` → klaviyo.com
3. `251.14.1.16` → mx3.personalyz.io

The bottom-most `Received:` entry shows the first hop from the sender to `gwagm.co`.

---

## ✅ Solution (Flag)
`252.44.98.29`

---

## 📌 Reflection
This was a basic email header tracing exercise. In a real incident response scenario I would also check SPF/DKIM/DMARC results and enrich the IP (WHOIS, passive DNS, threat feeds) to better understand the sender's reputation and possible infrastructure.
