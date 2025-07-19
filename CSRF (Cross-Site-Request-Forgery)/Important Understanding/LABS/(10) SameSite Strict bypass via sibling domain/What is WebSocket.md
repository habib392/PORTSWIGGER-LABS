## 📄 **File Title:** `Understanding WebSockets & CSWSH (in simple terms)`

---

# 🧠 WebSocket kya hota hai?

**WebSocket ek aisa connection hota hai jo browser aur server ke darmiyan continuously chalta rehta hai.**
Yeh normal HTTP jaise nahi hota jahan request bhejo aur phir response mile. Yeh ek **live pipeline** hoti hai jahan dono taraf data kisi bhi waqt bheja ja sakta hai — real-time.

---

### 🔁 WebSocket ka use kahan hota hai?

* Facebook Messenger (Live chat)
* WhatsApp Web
* Online games
* Stock price updates
* Live notifications (jaise "You got a new message")

---

# 🤝 WebSocket Handshake kya hota hai?

Jab browser WebSocket connection start karta hai, toh pehle ek **special HTTP request** bhejta hai jise **handshake** kehte hain.

### 🧾 Example Handshake Request:

```http
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: xyz123==
Origin: https://example.com
Cookie: sessionid=abcd1234
```

### 🔁 Server Response:

```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
```

**Agar response mein 101 status code mil jaye, iska matlab connection successfully WebSocket pe shift ho gaya.**

---

# 🍪 Cookie ka role kya hai?

* Jab user login hota hai, toh browser mein ek **session cookie** store hoti hai.
* WebSocket handshake mein yeh cookie bhi bheji jati hai.
* Server us cookie se pehchanta hai ke yeh kaun user hai.

Yani agar koi user Facebook pe login hai, toh WebSocket handshake ke time wo session cookie automatically chali jati hai — isliye user ko dobara login nahi karna padta.

---

# ⚔️ CSWSH kya hota hai? (Cross-Site WebSocket Hijacking)

CSWSH ek attack hota hai jahan:

1. Attacker ek **evil website** banata hai.
2. Victim (jo already kisi website pe login hai) us website ko visit karta hai.
3. **Browser apne login session ke cookies ke sath WebSocket handshake kar deta hai** attacker ke code ke through.
4. Attacker ab us user ka WebSocket hijack kar leta hai — bina usko pata chale.

---

### 😱 Real-Life Example:

1. User login hai `facebook.com` pe.
2. Attacker ki website `evil.com` pe jata hai user.
3. Attacker ka JS code silently ye karta hai:

```javascript
let ws = new WebSocket("wss://facebook.com/socket");
ws.send("withdrawFunds()");
```

4. User ki cookie ke sath handshake ho jata hai.
5. Facebook server samajhta hai ke yeh genuine user hai — aur paisa nikal bhi jata hai 😨

---

# 🛡️ CSWSH se bachav (defenses):

1. **Origin Header Check** karo handshake mein.
2. WebSocket mein **token-based auth** use karo — sirf cookie pe rely mat karo.
3. Session cookies mein `SameSite=Strict` lagao.
4. WebSocket messages ko **server-side validate** karo — har ek message!

---

# 🕵️‍♂️ Pentesting Tips (Burp Suite use karke):

* **DevTools > Network > WS** tab mein dekh WebSocket open ho raha ya nahi.
* Burp ka **WebSocket history** tab check kar.
* Handshake request mein `Origin` aur `Cookie` headers dhyan se dekh.
* Server response 101 ho toh samajh ja connection open ho gaya.
* Check kar koi **auth token** use ho raha hai ya sirf cookie?

---

# 🔍 Key Headers for WebSocket Handshake:

| Header                | Purpose                                 |
| --------------------- | --------------------------------------- |
| `Upgrade: websocket`  | HTTP se WebSocket shift karne ke liye   |
| `Connection: Upgrade` | Server ko batata hai ke upgrade chahiye |
| `Origin`              | Jahan se request aa rahi hai            |
| `Cookie`              | Auth info, session id etc.              |

---

# ✅ Final Note:

Jab bhi tu kisi modern web app ka pentest kare, toh **WebSocket traffic ko miss mat karna**. Aksar developers sirf frontend auth pe focus karte hain, jabke WebSocket endpoints unprotected chhor dete hain — **CSWSH wahi se hota hai.**

---
