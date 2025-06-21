# 🧠 Referer-based access control 
Kuch websites sirf Referer header dekh kar decide karti hain ke kisi user ko access dena chahiye ya nahi.

### 🔍 Referer header kya hota hai?
Jab browser kisi link pe click karta hai ya koi action hota hai, toh browser request ke sath ek header bhejta hai jisme likha hota hai ke yeh request kis page se aayi hai — isko Referer kehte hain.

### 🛑 Kya problem hoti hai?
Misaal ke taur pe:

/admin page pe to strict access control laga hota hai.

Lekin jab tum /admin/deleteUser pe jaate ho, toh woh sirf check karta hai ke Referer mein /admin likha hai ya nahi.

👉 Problem yeh hai ke attacker apni request mein Referer header fake kar sakta hai.

### 🧪 Exploit kaise hota hai?
Attacker /admin/deleteUser ka direct request banata hai.

Request mein fake Referer: https://vulnerable-website.com/admin add karta hai.

Server sirf Referer check karta hai aur samajhta hai ke yeh sahi page se aayi hai.

Attacker unauthorized action perform kar leta hai — jaise kisi user ko delete karna.

### 🕵️‍♂️ Penetration Testing Tip:
Jab bhi koi admin ya sensitive function ka URL mile (jaise /admin/deleteUser, /admin/editRole), toh Burp Suite mein request modify karo aur Referer header fake kar ke dekho — agar action ho jaata hai, toh yeh Referer-based access control flaw hai.
