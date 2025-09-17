# D2 — Not-so-Simple Mail Protocol 

**Platform:** CTF / Log Analysis  
**Category:** Email / SIEM / Log Analysis  
**Difficulty:** Medium

---

## 🎯 Goal
Find the log entry for an email sent to `security@personalyz.io` (date range narrowed to Mar 20–22) and extract relevant metadata: origin IP, mailfrom, subject, and path.

---

## 🧠 Approach
Filter logs by recipient (`security@personalyz.io`) and date range (Mar 20–22). Inspect fields like `id.orig_h`, `mailfrom`, `msg_id`, and `path` to determine origin and message metadata.

---

## 🛠️ Steps / Evidence
Found the matching log entry with these fields:

helo: tgwnaagm.co
date: Mar 21, 2025 @ 19:46:09.040
uid: C6ZURZvN41b73LzWUx
id.orig_h: 252.44.98.26
id.orig_p: 41569
id.resp_h: 245.31.211.53
id.resp_p: 25
mailfrom: tharris456@tgwnaagm.co

rcptto: security@personalyz.io

from: "Thomas Harris" tharris456@tgwnaagm.co

msg_id: 1742600769.040483@tgwnaagm.co

subject: Warning: We have acquired sensitive data
path: 252.44.98.26, 250.24.46.164, 245.31.211.53
user_agent: XyzMailerPro
blocked: true


---

## ✅ Findings
- **mailfrom:** `tharris456@tgwnaagm.co`  
- **Original IP (`id.orig_h`):** `252.44.98.26`  
- **Path:** `252.44.98.26, 250.24.46.164, 245.31.211.53`  
- **Subject:** `Warning: We have acquired sensitive data`

---

## 📌 Reflection
This exercise shows the value of targeted SIEM/log filtering (recipient + time window). In a production environment I'd enrich `id.orig_h` with WHOIS/passive DNS, look for related messages, and pivot to mail server logs for deeper context.
