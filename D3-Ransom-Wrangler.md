# D3 — Ransom Wrangler (100)

**Platform:** Interactive CTF  
**Category:** Social Engineering / Incident Response  
**Difficulty:** Medium

---

## 🎯 Goal
Interact with the attacker via the challenge mailbox to:
1. Obtain a sample of stolen data (Flag 1: an email address).
2. Negotiate the ransom to below 30 BTC (CTF-RAN-...).
3. Negotiate an extension of the deadline to 96 hours (CTF-DEA-...).

---

## 🧠 Approach
Use calm, professional language; request verification (a data sample) to confirm a real breach; use plausible internal constraints (legal/finance limits) to negotiate ransom and deadline.

---

## 🛠️ Steps / Evidence
1. Logged into the challenge mailbox and found the initial ransom message.  
2. Sent a professional request for a sample:


Hi,
Please provide a small sample of the allegedly exfiltrated data so we can verify the claim and proceed accordingly.
Thank you.

3. Attacker supplied a sample containing `bear.pigeon@hotmail.es` → **Flag 1**.  
4. Negotiated ransom using finance/legal constraints; after counteroffers the attacker accepted `29 BTC` and returned `CTF-RAN-1BB5E053`.  
5. Requested deadline extension to 96 hours citing internal approvals; after exchanges attacker agreed and returned `CTF-DEA-52DEB4E9`.

---

## ✅ Solution (Final Submission)
`bear.pigeon@hotmail.es:CTF-RAN-1BB5E053:CTF-DEA-52DEB4E9`

---

## 📌 Reflection
This challenge simulated real-world ransomware negotiation and documentation. Best practices include logging all correspondence, requesting proof before engagement, involving legal/finance, and exhausting negotiation options before escalation.
