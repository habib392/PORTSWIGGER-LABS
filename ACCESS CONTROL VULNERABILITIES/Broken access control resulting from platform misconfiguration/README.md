# 🔐 Broken Access Control from Platform Misconfiguration — Aasan Zuban Mein:

Kuch websites ya web apps aisi hoti hain jo access control ka kaam platform level pe karti hain. Matlab woh decide karti hain ke kaun sa user kaun se URL ko access kar sakta hai aur kaun sa method (jaise GET, POST) use kar sakta hai.

### 🔧 Example:

DENY: POST, /admin/deleteUser, managers

Iska matlab: Manager group ke users POST request nahi bhej sakte /admin/deleteUser wale URL pe.

### 😬 Masla Kahan Hota Hai?

Kabhi kabhi platform ya framework kuch non-standard headers allow karta hai jaise:

- X-Original-URL

- X-Rewrite-URL

- X-Forwarded-For

Yeh headers request ke original URL ko override (badal) kar sakte hain.

To agar front-end (browser side) pe URL block kiya gaya ho lekin backend (server) pe yeh headers allow hon, to hacker asaani se block ki gayi functionality tak pohanch sakta hai.

### 🧠 Real Example:

Agar app normally yeh block kar rahy ho:

**POST /admin/deleteUser**

To attacker aisi request bhej sakta hai:

**POST / HTTP/1.1  
X-Original-URL: /admin/deleteUser**

Ab server yeh samjhta hai ke request /admin/deleteUser ke liye hai, aur attacker ne restriction bypass kar liya!

## 🛡️ Penetration Testing Tip:

Jab bhi tum kisi site ki testing karo, to non-standard headers test karo:

- X-Original-URL

- X-Rewrite-URL

- X-Custom-IP-Authorization

- X-Forwarded-For

Dekho kya server in headers ko treat karta hai. Agar karta hai, to Access Control Bypass ka chance hota hai. Yeh critical vulnerability ho sakti hai.