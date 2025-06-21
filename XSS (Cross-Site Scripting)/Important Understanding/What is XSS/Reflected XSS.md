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