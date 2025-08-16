## QUESTIONS / ANSWERS

**Question.** Matlab Server ny aik whitelist lagahi jis main sirf aik url jaisy weliketoshop.net ki request accept
hoti thi matlab server sirf iss url sy stock ka data ly rha tha iss liye hum ny @# ky baad iss url ko
daala takay iss url ki wajah sy hamara overall localhost wala url bhi accept hoo jaye or server iss pr request bhej kr hamy data laa dy

**Ans.** Bilkul developer ne whitelist banayi thi ke sirf `stock.weliketoshop.net` allow hai, aur woh sirf hostname check kar raha tha. Humne `@` aur `#` ka combination use karke **URL parsing ka loophole** exploit kiya.

Server ne hostname part ko whitelist se match kiya (`weliketoshop.net` ✔️) lekin actual request jo backend ne bheji woh `localhost` par chali gayi (kyunki `%2523` ne URL parsing ko confuse kar diya). Is tarah humne localhost ka **admin panel access** kar liya aur Carlos ko delete kar diya.

### ⚡ Simple Line me

**Whitelist → Sirf hostname check hota tha → Encoding + @ trick se server confuse → Request localhost pe gayi → Admin access mil gaya.**

---

### **1. URL Parsing kya hota hai?**

Jab server ko ek URL milta hai (jaise `http://username:password@hostname:port/path?query#fragment`) to woh usko **tod kar alag alag parts** me samajhta hai:

* **Protocol:** `http://`
* **Username/Password (optional):** `username:password@`
* **Hostname:** `hostname`
* **Port:** `:80` (default agar nahi likha to bhi assume hota hai)
* **Path:** `/path`
* **Query:** `?query`
* **Fragment:** `#fragment`

👉 is process ko hi **URL parsing** kehte hain.

---

### **2. Loophole kya hota hai?**

Loophole ka matlab hai **coding ya design ki chhoti si galti** jahan developer ka intention kuch aur hota hai lekin code behave kuch aur karta hai.

Yahan loophole ye tha:

* Server whitelist check kar raha tha sirf hostname par (`stock.weliketoshop.net`).
* Lekin humne encoding aur `@` use karke server ko confuse kar diya → socha hostname sahi hai, lekin asal request `localhost` pe chali gayi.

---

### **3. Localhost ke sath 80 kyun likha?**

`localhost` ke sath `:80` likhne ka reason ye hai ke:

* **Port 80** web servers ka default HTTP port hota hai.
* Agar tum bas `http://localhost/` likho to bhi woh by default port 80 pe hi connect karega.
* Lekin humne exploit me `localhost:80%2523@stock.weliketoshop.net` likha taake server ko **aur zyada exact lagay** aur whitelist bypass ho jaye.

---

⚡ **Ek line me:**

* **URL parsing** = server URL ko tod tod ke samajhta hai.
* **Loophole** = coding/design ki galti jahan exploit possible ho.
* **Port 80** = default web server port (HTTP ke liye).

---

