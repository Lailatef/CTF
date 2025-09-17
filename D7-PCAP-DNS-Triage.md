# D7 — PCAP Triage / DNS Analysis 

**Platform:** Network Forensics (Wireshark)  
**Category:** PCAP Analysis / DNS Forensics  
**Difficulty:** Medium

---

## 🎯 Goal
Identify DNS queries to the C2 IP (`251.91.13.37`) and extract encoded payloads or suspicious query names for decoding.

---

## 🧠 Approach
Use Wireshark display filters to isolate DNS traffic to/from the C2 IP and extract DNS query names for further decoding.

Key filters used:
- `ip.dst == 251.91.13.37 && dns`
- `ip.dst == 251.91.13.37 and dns.qry.name`
- `ip.src == 10.75.34.13 && ip.dst == 251.91.13.37 && dns.flags.response == 0`

---

## 🛠️ Steps / Evidence
- Isolated DNS queries to `251.91.13.37` and observed long encoded query names (indicative of exfiltration).  
- Extracted encoded tokens and decoded them (see D6) to recover PII and victim records.

---

## ✅ Findings
- Repetitive large DNS queries to `251.91.13.37` carrying base32-encoded payloads.  
- Decoded payloads contained personal data (see D6).

---

## 📌 Reflection
Large, repetitive DNS query labels are a common pattern for data exfiltration. Wireshark + filters are effective to quickly triage and extract candidate strings for decoding.
