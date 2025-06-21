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