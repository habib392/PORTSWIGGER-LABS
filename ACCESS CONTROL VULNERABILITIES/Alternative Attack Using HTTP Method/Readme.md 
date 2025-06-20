# 🧨 Alternative Attack Using HTTP Method 

Jaise pehle samjha tha, kuch websites URL aur HTTP method (jaise POST, GET, PUT, etc.) ko use karti hain access control ke liye.

Matlab:

“Yeh URL sirf POST request se kaam karega, aur sirf admin hi access kar sakta hai.”

### 😈 Hacker Kya Karta Hai?

Kuch websites sirf URL check karti hain, method ka dhyan nahi rakhti properly.
To agar front-end ya server POST method hi required hai kisi action ke liye (jaise /admin/deleteUser),
aur hacker GET method se wohi kaam kar le, to control bypass ho gaya.

### 🧠 Real Example:

Normal expected:

**POST /admin/deleteUser**

(Only admin POST kar sakta hai)

Attacker ne yeh try kiya:

**GET /admin/deleteUser**

Aur agar server ne GET ko bhi allow kar diya (galti se), to attacker woh action kar lega — bina permission ke

## 📚 Bonus Tip: Method Tampering aur bhi try kar:

**HEAD /admin/deleteUser HTTP/1.1**

**PUT /admin/deleteUser HTTP/1.1**

**DELETE /admin/deleteUser HTTP/1.1**

Kuch servers inko bhi galti se allow kar dete hain. Bas har request pe response ka status code (200, 403, 500) dekhna — samajh aa jaye ga kya leak ho raha hai

### 🛡️ Penetration Testing Tip:

Jab bhi tu access control test kare:

✅ GET, POST, PUT, DELETE sab methods use kar ke dekh

✅ Har method se same URL access karke check kar kya hota hai

✅ Agar kuch extra mil jaye — toh method-based bypass mil gaya 😎