# Yeh strategy kab kaam aati hai?

✅ Jab:

1. Password reset email mein jo URL aata hai, wo backend par Host header ya X-Forwarded-Host se generate hota hai.


2. Application user input ya headers pe trust kar rahi hoti hai bina verify kiye.


3. Victim email ka link click karta hai bina check kiye (jaise Carlos).


4. Exploit server available ho taake tu URL ko redirect kar sake apni taraf.


## 🧪 Real World Example:

Suppose tu ek bug bounty program mein ho, aur website ka forgot password flow aise kaam karta hai:

1. Tu password reset request bhejta hai victim ke liye (jaise carlos).


2. Tu header add karta hai:
X-Forwarded-Host: attacker-site.com


3. Website usi header se email mein link bana ke victim ko bhej deti hai: https://attacker-site.com/reset-password?token=abc123

4. Victim click karta hai → tu uska token grab kar leta hai via logs.

5. Token ka use kar ke tu uska password reset kar leta hai.

## 💡 Penetration Testing Tip:

Jab bhi tu password reset functionality test kare, yeh 4 cheezein check kar:

🔎 Check	Usefulness in Pentest

X-Forwarded-Host supported hai?	Exploitable reset poisoning
Email mein URL kis header se banta hai?	Identify injection point
Victim token link tu receive kar sakta hai?	Exploit possible
Token expire hota hai ya nahi?	Token reuse vulnerability

### 📌 Summary:

Tu yeh attack tab use karega jab tu kisi web app ka password reset mechanism test kar raha ho, especially jab:

Tu bug bounty, client penetration test, ya CTF kar raha ho.

Tu dekh raha ho ke email wali reset link kis tarah generate hoti hai.

Tu Host ya X-Forwarded-Host inject kar ke apni URL bitha sakta hai.