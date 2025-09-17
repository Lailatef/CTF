# D5 — Exfiltration / Process Triage 

**Platform:** Forensics / Host Triage  
**Category:** Host Forensics / Process + Network Analysis  
**Difficulty:** Medium

---

## 🎯 Goal
Using `lsof.txt`, `ps.txt`, and `history.txt`, identify the user, real process name, and PID responsible for exfiltration to `251.91.13.37`. Produce the flag in the format `user_process_pid`.

---

## 🧠 Approach
1. Search `lsof.txt` for connections to `251.91.13.37` to find USER and PID.  
2. Cross-reference PID in `ps.txt` to see the running command.  
3. Inspect `history.txt` to discover if the binary was renamed or symlinked (revealing the true malicious name).

---

## 🛠️ Steps / Evidence
From `lsof.txt`:

64866 backupsys 45u IPv4 ... UDP 10.75.34.13:33421->251.91.13.37:dns (ESTABLISHED)

→ **USER:** `backupsys`, **PID:** `64866`.

From `ps.txt`:

backupsys 64866 ... /usr/bin/jot

→ Process name shown as `/usr/bin/jot`.

From `history.txt`:

sudo rm /usr/bin/jot
ln -s /usr/bin/jot /usr/.local/bin/backupy
/usr/bin/jot

→ Shows `/usr/bin/jot` was removed and a symlink created linking `/usr/bin/jot` to `/usr/.local/bin/backupy`, indicating the actual exfil tool name is `backupy`.

---

## ✅ Solution (Flag)
`backupsys_backupy_64866`

---

## 📌 Reflection
This is a classic triage flow: network socket → process → command history. The attacker used a symlink/rename to disguise the payload; `history` exposed the renamed tool. In production, an analyst would collect memory, file timestamps, and preserve evidence before remediation.


