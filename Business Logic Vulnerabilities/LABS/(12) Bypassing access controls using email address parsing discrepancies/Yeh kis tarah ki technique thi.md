### 🧠 **Yeh kis tarah ki technique thi?**

**➡️ Technique ka naam:**
**Email Address Parsing Discrepancy / Encoded Email Injection**

**➡️ Category:**
**Access Control Bypass** + **Input Encoding Exploit**

**➡️ Kaise kaam karti hai:**

* Frontend validation aur backend parsing **alag-alag parser/libraries** use karte hain
* Attacker email ko aise format mein encode karta hai (jaise **UTF-7**, **RFC-2047 encoded-word**)
* Server ko lagta hai email safe hai (`@company.com`)
* Lekin actual email send hota hai attacker ke domain par (`@attacker.com`)

> Yani **human ko aur machine ko alag cheez dikhayi ja rahi hai** — isi ko parser discrepancy bolte hain.

---

### 📅 **Kya aaj kal yeh vulnerability milti hai?**

**✅ Haan, milti hai — lekin rare hai.**

**⚠️ Kyunke:**

* Modern frameworks aur email libraries in encoding tricks ko block karne lage hain
* Lekin **legacy systems**, **custom email validators**, aur **microservices architecture** mein ab bhi yeh bugs mil jate hain

**💡 Real-world example:**
2021 mein Gareth Heyes (PortSwigger) ne ek full whitepaper publish kiya tha:

> **"Splitting the Email Atom"**
> Jis mein unho ne bataya ke Gmail, Microsoft Outlook, aur kai systems yeh encodings differently parse karte hain — aur attackers iska faida uthate hain.

---

### 🎯 Penetration Tester ke liye kya faida?

* Jab email domain restricted ho (`@admin.com`) — to yeh technique kaam aa sakti hai
* Tum phishing, privilege escalation ya fake admin accounts bana sakte ho
* Yani **authorization bypass** without SQLi or direct auth bugs


