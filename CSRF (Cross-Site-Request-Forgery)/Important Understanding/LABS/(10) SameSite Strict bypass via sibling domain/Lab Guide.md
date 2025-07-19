**SameSite restriction ka bypass sibling domains ke zariye kaise hota hai:**

Agar tum kisi website ka security test kar rahe ho (ya apni hi secure kar rahe ho), toh ek baat yaad rakhna: **agar request kisi aur origin se ho, lekin woh origin usi website ke group (yaani sibling domain) mein ho, toh browser usko "same-site" maan sakta hai.**

**Example:**
`admin.example.com` aur `shop.example.com` — dono alag origin hain, lekin same site ke domains hain.

Aise cases mein agar kisi sibling domain (jaise `shop.example.com`) mein koi **XSS ya injection** wali vulnerability ho, toh attacker uska faida utha ke **kisi doosre domain** (jaise `admin.example.com`) ko bhi target kar sakta hai. Isse site ke saare defense fail ho jate hain — **CSRF bhi ho sakta hai, aur session hijack bhi.**

Agar website WebSockets use kar rahi ho, toh aur bhi khatra hota hai — attacker **WebSocket handshake** ke zariye bhi attack kar sakta hai, jisko **CSWSH (Cross-Site WebSocket Hijacking)** kehte hain. Yeh bhi ek tarah ka CSRF hi hota hai, lekin WebSocket ka version.

---

