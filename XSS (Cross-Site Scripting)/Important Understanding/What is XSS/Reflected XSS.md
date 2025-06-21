# 💡 Reflected XSS 

Reflected XSS tab hota hai jab website kisi user ka diya hua data URL se leti hai, aur usi waqt bina check kiye usko page par wapas dikhati hai.

### 🧪 Example samjho:

Tumhari ek website ka search function hai. Tum search karte ho “gift”, tou URL ban jata hai:

**https://insecure-website.com/search?term=gift**

Aur page pe aise dikhta hai:

**<p>You searched for: gift</p>**

Ab attacker isi cheez ka faida uthata hai. Wo “gift” ki jagah JavaScript daal deta hai:

**https://insecure-website.com/search?term=<script>alert('XSS')</script>**

Ab page ka response hoga:

**<p>You searched for: <script>alert('XSS')</script></p>**

Yani ab jab koi bhi banda attacker ka diya hua link open karega, uska browser turant wo JavaScript chala dega — aur alert pop-up show hoga.

### 😈 Iska nuksan kya hai?

Agar victim user already logged in hai, tou attacker uski:

- Session cookies chura sakta hai

- Uske naam pe koi bhi action perform kar sakta hai

- Sensitive info access kar sakta hai

Sab kuch user ke browser main hota hai, aur victim ko shayad kuch pata bhi na chale.

#💣 Reflected XSS ka Asar

Agar attacker ka script victim ke browser mein chal jaye, tou uska matlab hai ke attacker us user ka pura control le sakta hai — jaise:

User ki tarah koi bhi action kar sakta hai (jaise delete karna, update karna, form submit karna)

Jo kuch user dekh sakta hai, attacker bhi dekh sakta hai

Jo data user change kar sakta hai, attacker bhi change kar sakta hai

Dusre users ke sath bhi malicious interaction kar sakta hai — jaise fake message ya attack bhejna

### 💌 Attack deliver kaise hota hai?

Attacker link bana kar:

Apni website par daal sakta hai

Kisi forum/post/comment jahan content allowed ho wahan chipka sakta hai

Victim ko email, tweet, WhatsApp ya kisi message ke through bhej sakta hai

Ye link targeted bhi ho sakta hai (jaise kisi admin ke liye) ya public bhi ho sakta hai (sab logon ke liye).


### 🔍 Reflected XSS vs Stored XSS

Reflected XSS mein attacker ko link send karna padta hai — yaani victim ko kisi na kisi tarah se us link pe click karwana hota hai.

Lekin Stored XSS mein attacker ka code website ke andar hi permanently hota hai — victim ko kuch click karne ki bhi zarurat nahi hoti.

📌 Is liye Reflected XSS ka impact thoda kam hota hai Stored XSS ke mukable mein.

# 🎯 Reflected XSS — Different Contexts Mein

Reflected XSS ki kai alag alag types hoti hain — aur yeh depend karta hai ke:

1. Website ka response mein attacker ka input kahaan reflect ho raha hai

2. Aur kis form mein reflect ho raha hai

### 📍 Kya matlab hai "kahaan reflect ho raha hai"?

Yani:

- Agar input HTML tag ke andar hai, tou alag payload chahiye

- Agar input JavaScript block ke andar hai, tou payload different hoga

- Agar input attribute (jaise src ya href) mein hai, tou payload or alag banega

Yeh location decide karti hai ke kaunsa payload kaam karega — aur vulnerability ka impact bhi is pe depend karta hai.

### 🛡️ Data par processing ka asar

Agar website tumhare input pe koi validation ya filtering lagati hai (jaise < ya script remove kar deti hai), tou tumhe smart payload banana padta hai — jo us filter ko bypass kare.

Is liye reflected XSS ka success context + filtering dono cheezon par depend karta hai.

# 🕵️‍♂️ Reflected XSS Dhoondhne Aur Test Karne Ka Tarika – Tumhari Zuban Main

### ✅ 1. Burp Suite Scanner ka Use

Agar tumhare paas Burp Suite Professional hai, tou uska scanner automatically reflected XSS vulnerabilities ko fast aur reliably dhoond leta hai.

Lekin agar manual testing kar rahe ho (jo seekhne ke liye best hai), tou ye steps follow karo:

### 🔍 2. Har Input Point Test Karo

App ke har jagah jahan user input jaa sakta hai — jaise:

- URL parameters (e.g. ?search=abc)

- POST body

- URL path (e.g. /profile/username)

- HTTP headers (jaise User-Agent, Referer)

Sab ko alag alag test karo, kyunke har jagah alag context hoti hai.

### 🧪 3. Random Value Inject Karo

Sab jagah random 8-character alphanumeric (jaise a7x9p3d2) value daalo — taake pata chale kaha reflect ho rahi hai.

Burp Intruder se aise random values automatically generate kar sakte ho.
Burp ka grep match feature use karo taake woh automatically bata de response mein value nazar aayi ya nahi.

### 🌍 4. Context Samjho

Jahan value reflect ho rahi hai, uska context dekhna zaroori hai:

- HTML body main?

- HTML tag ke andar?

- JavaScript ke andar?

- Attribute ke andar?

Ye context decide karega ke kaunsa payload kaam karega.

# 💥 5. Candidate Payload Test Karo

Ab ek simple payload test karo jaise:

**><script>alert(1)</script>**

Burp Repeater mein request bhejo, response check karo — agar payload as-it-is reflect ho gaya, to alert chal sakta hai.

### Tip: Random value ko payload ke sath rakhna — jese:

**test123<script>alert(1)</script>**

Phir Burp Repeater mein test123 ko search karo, taake tum jaldi se location trace kar sako.

### 🔄 6. Agar Payload Block Ho Gaya To...

Agar website payload ko modify ya block kar rahi hai, to alternate payloads try karo — jaise encoded, broken up payloads, etc.

Jaise:

**><img src=x onerror=alert(1)>**

Yeh context aur filtering ke hisaab se change hota hai — PortSwigger ka “contexts” section helpful hai ismein.

### 🌐 7. Browser Mein Final Test

Agar Burp Repeater mein payload reflect ho raha hai, to browser mein real test karo — URL ko paste karo aur dekho alert chalta hai ya nahi.

Test payload ke liye:

**alert(document.domain)**

Agar popup aaya, to attack successful hai ✅