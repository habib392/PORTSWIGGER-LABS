# 🔥 Cross-site Scripting (XSS)

### XSS kya hota hai?
Cross-site scripting yaani XSS aik web security vulnerability hai jo kisi website main hoti hai. Agar website vulnerable ho, tou attacker us site ko aise manipulate kar sakta hai ke jab koi user us site ko visit kare, tou uske browser main attacker ka JavaScript code chal jaye.

Yani website to lag rahi hoti hai normal, lekin andar se attacker ka code user ke browser main silently run ho raha hota hai.

### 🚨 XSS se attacker kya kar sakta hai?

Victim user ban kar website use kar sakta hai.

User ke sensitive data (jaise cookies, tokens) le sakta hai.

Agar user admin hai tou poori website ka control le sakta hai.

### 🧠 XSS kaam kaise karta hai?

Attacker website main aise input inject karta hai jisme JavaScript hoti hai (jaise form, URL, comment box waghera). Agar website input ko clean nahi karti, tou ye malicious JavaScript sab users ke browser main run hoti hai.

Jab ye JavaScript user ke browser main chalti hai, tou attacker us user ka data chura sakta hai ya actions perform kar sakta hai jaise user khud kar raha ho.

### 🛡️ Penetration Testing ke liye iska use kya hai?

Penetration tester XSS use karke:

Users ka session hijack kar sakta hai (session fixation / cookie theft).

Admin panel ka access le sakta hai agar victim admin ho.

Fake login forms ya phishing traps create kar sakta hai.

Keylogger inject kar sakta hai taake user ke credentials mil jayein.

### 🧪 Real World Example:

Agar tum kisi blog comment section main **<script>alert('Hacked')</script>** likh do aur woh site isko bina sanitize kiye dikha de — tou XSS ho gaya.