# Flawed 2FA Verification Logic – Asaan Lafzon Mein

Maan le:

Website ka 2-step login hai:

- 1. Pehle username & password
- 2. Phir OTP (2FA code)

Lekin website ka system itna bewakoof hai ke:

Pehle step mein jab user login karta hai, usay ek cookie milti hai:

Set-Cookie: account=carlos

Phir doosray step (OTP wale page) pe sirf cookie dekh ke decide karta hai ke kis user ka OTP verify karna hai.

## 🧠 Toh flaw kya hai?

### Attacker:

1. Apne account (habibawan) se login karta hai

2. Usay cookie milti hai: account=habibawan

3. Jab OTP submit karne ka time aata hai, attacker:

OTP randomly brute-force karta hai

Lekin cookie ko change kar deta hai:

Cookie: account=victim-user

➡️ Server yeh check hi nahi karta ke pehle step kis user ne kiya tha — woh bas cookie padhta hai
➡️ Iss wajah se attacker OTP brute-force karke kisi bhi user ke account mein ghus sakta hai — bina password jaane!


## 🔓 Real Life Mein Kya Matlab?

Agar website sirf cookie dekh kar OTP verify kare:

Toh attacker password ki zarurat hi nahi

Sirf victim ka username pata ho, aur OTP guess karle (e.g., 6-digit codes)

Fir woh kisi ka bhi account access kar sakta hai

### 🔥 Attack Flow Summary:

1. Attacker login karta hai apne credentials se

2. Server usay cookie deta hai: account=attacker

3. OTP page pe attacker:

Cookie change karta hai: account=carlos

Brute-force se OTP guess karta hai

4. Agar OTP match hogaya — ✅ Carlos ka account access ho gaya!
