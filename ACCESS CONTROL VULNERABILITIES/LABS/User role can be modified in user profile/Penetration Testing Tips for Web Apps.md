# Penetration Testing Tips for Web Apps
### 1. 🧭 Har Web App ka Flow Samjho Pehle
Pehlay samjho: Login → Profile → Admin → API → Logout

Fir user roles, input points, URLs ko observe karo

✅ Example: /admin, /api/user, /profile/edit

### 2. 🔎 Burp Suite ka HTTP History Tab Use Karo


Har request-response observe karo — headers, cookies, params
✅ Common issues: isAdmin=true, roleid=1, user=123

### 3. 🧪 Manual Testing Zyada Powerful Hai

Extensions aur scanners helpful hain, lekin manual fuzzing (Repeater, Intruder) zyada bugs nikaalti hai

✅ Use Burp Repeater and Intruder often

### 4. 🧠 Think Like a Lazy Developer

“Agar developer ne ye check miss kar diya ho to?”

Har input mein socho: kya yeh bypass ho sakta hai?

✅ e.g., Hidden field change, cookie modify, ID tamper

### 5. 🧰 Input Tampering is Your Weapon

URL params, form fields, headers, cookies — sab kuch test karo

✅ Example:

POST /profile  

{ "user":"wiener" } → change to "carlos"

### 6. 🧾 Common Attack Vectors Yaad Rakho

SQLi

XSS (Reflected + Stored)

IDOR (Insecure Direct Object Reference)

CSRF

Broken Auth (Login bypass, password reset token abuse)

File Upload Bypass

### 7. 🔁 Repeat Payloads Har Jagah
Jahan input ho, wahin payload try karo:

✅ '><script>alert(1)</script> (XSS)

✅ ' OR 1=1-- (SQLi)

✅ ../../../../etc/passwd (LFI)

### 8. 🔐 Session aur Cookie Ko Analyze Karo
Check karo:

Set-Cookie headers

session, auth, admin, token

Inme tampering se kuch change hota hai kya?

### 9. 🧠 Always Check Server-side Validation
Jo frontend allow nahi karta, backend shayad allow kar de

✅ Example: HTML form readonly → Inspect Element → Remove attribute → Modify & Submit

### 10. 📖 OWASP Top 10 Ka Deep Study Karo
PortSwigger labs mostly OWASP Top 10 pe based hain

✅ Har lab ko is nazar se dekho: yeh kis OWASP category mein fall karta hai?

### Bonus Tip 🎯
Burp Suite ke “Logger++” aur “Turbo Intruder” jese extensions bhi try karo jab tu advance pe pahuche.

