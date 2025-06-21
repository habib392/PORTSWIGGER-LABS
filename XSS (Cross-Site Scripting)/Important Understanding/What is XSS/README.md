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

# 🧪 XSS Proof of Concept

Jab bhi tum check karna chaho ke kisi website main XSS vulnerability hai ya nahi, tou tum ek chhoti si JavaScript payload inject karte ho jo tumhare browser main chalti hai.

Sabse common aur simple payload hai:

**alert(1)**

Ye **alert()** function sirf ek chhoti pop-up box dikhata hai — isse koi nuksan nahi hota, aur jab ye box screen pe aata hai, tou tumhe pakka pata chal jata hai ke XSS ho gaya hai.

PortSwigger ke zyada labs bhi isi **alert()** se solve hote hain.


❗ Lekin Chrome ka ek masla

Chrome browser version 92 (July 2021 ke baad) se cross-origin iframes ke andar **alert()** chalna band ho gaya hai.

Aur kyunke kuch advanced labs iframe use karte hain, is liye **alert()** kaam nahi karta unme.

✅ Alternate: **print()**

Ab jab **alert()** fail ho jaye, tou tum **print()** use kar sakte ho — ye bhi JavaScript ka function hai jo screen pe kuch output deta hai.

PortSwigger ne unhi labs ko update kar diya hai jahan **alert()** kaam nahi karta, taake tum **print()** se bhi unhe solve kar sako.

Agar kisi lab main **print()** use karna ho, tou woh instructions main likha hota hai.


# 🧠 XSS ke Mukhtalif Types

XSS attacks ke 3 main types hotay hain:

### 1️⃣ Reflected XSS

Isme malicious script user ke HTTP request se aati hai — jaise URL ya search box ke zariye.
Jab user woh specially crafted link open karta hai, tou script usi waqt reflect hoti hai aur browser main chalti hai.

### 📌 Example:
Tum kisi ko ek link bhejtay ho:
**http://example.com/search?q=<script>alert(1)</script>**
Agar woh banda link open kare, tou uska browser turant alert box dikha dega.

### 2️⃣ Stored XSS

Isme attacker apna JavaScript code website ke database main save kar deta hai — jaise comments, posts, ya profile fields main.

Jab koi bhi user woh page kholta hai jahan ye data show hota hai, script uske browser main automatically chal jati hai.

### 📌 Example:
Tum blog post pe ek comment likhtay ho:
**<script>alert('Hacked')</script>**
Aur jab log woh post dekhte hain, sab ke browsers main alert aata hai.

### 3️⃣ DOM-based XSS

Ye thoda advanced hota hai. Isme issue client-side JavaScript code main hota hai, na ke server-side pe.
Yani browser ka JavaScript khud hi input le kar manipulate karta hai aur vulnerable hota hai.

### 📌 Example:
Website ka JavaScript URL se data le kar directly page pe show kar raha ho — bina sanitize kiye. Attacker aise input de kar us code ko manipulate karta hai.