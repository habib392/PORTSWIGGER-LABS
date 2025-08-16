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

### 🔹 4. `#` ki importance kya hai?

* URL me `#` ko **fragment identifier** kehte hain.
* Fragment ka kaam hota hai **sirf browser ke andar page ke ek section tak jump karna** (jaise `page.html#section1`).
* **Important point:** jab URL server ko bheja jata hai, usme `#` aur uske baad ka part **server tak jata hi nahi**.

👉 Matlab agar hum likhen:

```
http://localhost#@stock.weliketoshop.net/
```

To server samjhega: hostname = `localhost`
Lekin browser/server ki parsing ke dauran `#` ke baad wala part **ignore ho jata hai**.

---

### 🔹 5. Double encoding `%2523` kiun use kiya?

* `%23` = ek baar encoding (means `#`)
* `%2523` = double encoding

  * `%25` = `%`
  * `%2523` = `%23` (jo phir decode hoke `#` banta hai)

Yahan trick yeh hai:

* **Whitelist check karne wala parser** sirf hostname extract karta hai aur `%2523` ko as string treat karta hai → result `stock.weliketoshop.net`.
* **Backend connection banate waqt** URL ek dafa decode hota hai → `%2523` → `%23` → `#`.
* Ab URL `localhost#@stock.weliketoshop.net` ban jata hai.
* `#` ki wajah se hostname ka part `localhost` ban jata hai → aur request localhost pe chali jaati hai.

👉 Matlab: **double encoding confuse karta hai do alag parsing layers ko.**

---

### 🔹 6. `localhost` ya `stock.weliketoshop.net` ko double encode karne se error kyun?

* Agar tum `localhost` ko `%256cocalhost` type encode kar do → parsing me woh invalid ban jaata hai → isliye error aata hai.
* Yeh trick sirf un reserved characters pe kaam karti hai jo parsing me **special meaning rakhte hain** (jese `#`, `?`, `/`, `@`).
* Normal words encode karne se URL toot jata hai.

---

### 🔹 7. `@` ki kya role hai?

* URL me `@` ka matlab hai → `username:password@hostname`.
* Matlab jo part `@` se pehle likha hai woh **credentials** hai, aur jo baad me likha hai woh **hostname** hai.
* Humne `localhost:80%2523@stock.weliketoshop.net` likha.

  * Whitelist ne hostname dekha = `stock.weliketoshop.net` (✔️ allowed).
  * Lekin backend ne jab decode kiya to `%2523` → `#` → hostname actually `localhost` ban gaya.

👉 **@ isliye encode nahi kiya** kyunki hume intentionally chahiye tha ke woh URL ka delimiter ki tarah treat ho, taake parsing alag alag jagah pe confuse ho jaye.

---

### ⚡ Root Level Summary:

* **`#`**: server ko URL ka part chhupa deta hai (fragment).
* **Double encoding `%2523`**: ek parser ke liye harmless string, doosre parser ke liye fragment → confusion = loophole.
* **`@`**: URL credentials delimiter → whitelist ko `stock.weliketoshop.net` dikhata hai, lekin actual request `localhost` pe jaati hai.

---

### 🔹 Technique ka Naam

Yeh technique ek **Whitelist Bypass using URL Parsing Confusion** hai.
Matlab developer ne ek whitelist banayi thi jisme sirf `stock.weliketoshop.net` allow tha. Humne URL encoding aur special characters (`@`, `#`) ka use karke server ko confuse kar diya aur request ko **localhost** par bhej diya.

---

### 🔹 Lab ki Khaas Baat

1. Normal SSRF se reject kar raha tha (kyunki whitelist lagi thi).
2. Trick thi `@` aur double encoding of `#` → isne parsing confuse kar di.
3. Whitelist dikh rahi thi correct (`stock.weliketoshop.net`) lekin asal request `localhost` pe chali gayi.
4. Yeh ek **classic real-world bypass example** hai jahan input validation bas surface level hoti hai.

---

### 🔹 Main Points of Lab

* Server me stock check feature tha jo backend URL call karta tha.
* Directly `localhost` dene se reject ho gaya.
* Whitelist sirf hostname check kar rahi thi → `stock.weliketoshop.net`.
* `@` ka use karke hostname ko split kiya: `localhost:80%2523@stock.weliketoshop.net`.
* Double encoding `%2523` ne whitelist parser aur backend parser ko confuse kiya.
* Final exploit:

  ```
  http://localhost:80%2523@stock.weliketoshop.net/admin/delete?username=carlos
  ```
* Carlos delete ho gaya → Lab Solved ✅

---

### 🔹 Whitelist kaise pata chala?

* Jab humne `http://127.0.0.1/` try kiya to reject kar diya.
* Lekin `http://stock.weliketoshop.net/` accept ho gaya.
* Matlab developer ne is hostname ko whitelist me daala tha.

**Real websites me kaise pata chalega?**

* Agar kuch URLs reject ho jayein aur ek specific domain hi kaam kare to samajh jao whitelist hai.
* Error messages, “only internal domains allowed”, ya “invalid host” type response se idea milta hai.
* Thoda fuzzing karke clear ho jata hai kaunse hosts allow hain.

---

### 🔹 Real-World Existence

Haan, yeh wali vulnerability aaj bhi milti hai. Kyunki:

* Developers mostly whitelist banate hain → lekin URL parsing libraries har language me alag behave karti hain.
* Agar encoding ya special characters ka test na kiya ho to bypass ho jata hai.

**Examples:**

* Cloud services me SSRF (AWS metadata bypass).
* Internal admin panels expose ho jaate hain isi trick se.

---

### 🔹 Developer ki Wajah se Hoti Hai?

100% haan.

* Developer samajhta hai ke whitelist = safe, lekin usne **different URL parsers ka behavior** consider hi nahi kiya.
* Ek parser whitelist check karta hai, doosra request banata hai. Yeh mismatch hi loophole ban jaata hai.

---

## ⚡ Root Level Summary

* **Technique:** Whitelist Bypass (URL parsing + encoding trick).
* **Khaas Baat:** Double encoding + `@` use karke server ko confuse karna.
* **Main Points:** Whitelist detect karna, encoding try karna, localhost access lena.
* **Whitelist Detection:** Trial and error se → sirf ek domain accept hota hai.
* **Real World:** Aaj bhi exist karti hai, cloud aur internal apps me.
* **Developer Fault:** Poor input validation aur multiple parsers ke alag behavior ignore karna.


