`|` ko **pipe operator** kehte hain.

Iska kaam hai:
👉 **Ek command ka output, agle command ka input bana do**
Ya simple case main: **do commands ek saath chala do**.

Example dekh:

```
ls | wc -l
```

* `ls` files list karega
* `wc -l` unki counting karega
* `|` dono ko connect karega

---

Ab lab wale case main:

Server asal main ye command chala raha tha (samajhne ke liye):

```
stockchecker 1
```

Tumne isko badal diya:

```
stockchecker 1 | whoami
```

➡ Matlab system pehle `stockchecker 1` run karega, phir uska output **pipe karke** `whoami` ke saath bhi chalega.
➡ Result: Tumhein response main **whoami ka output** mil gaya (jaise `www-data`).

---

💡 Command injection main agar `;` aur `&` block ho gaye hoon, to kabhi kabhi `|` kaam karta hai kyunki wo ek **valid command separator** bhi ban jata hai.

---

Jo **peter** output main mila na, woh **operating system ka naam nahi hai**.

Woh asal main us server ka **current system user** hai jo website ka backend process chala raha hai.

---

### Samajh le simple words main:

* Har operating system (Linux/Windows) pe commands kisi **user account** ke under chalti hain.
* Jaise Windows main tu `Administrator`, `Habib`, ya `Guest` ke user naam dekh sakta hai.
* Linux server pe normally user hota hai `www-data`, `apache`, `nobody`, ya kabhi custom naam (yahan unhone **peter** rakha hua hai).

---

### Matlab:

👉 `whoami` ne bataya ke jo web application chal rahi hai uske backend OS process ko **"peter" user** ke rights mile hue hain.
👉 Iska faida penetration tester ke liye ye hai ke ab tu janta hai:

* Agar yeh user **low privilege** hai, tu privilege escalation try karega.
* Agar yeh user root nikla, to tu seedha system ke full control main aa gaya.

---

💡 Real world main `whoami` chalane ke baad hamesha `id` aur `uname -a` jaise commands try karte hain taake system ke bare main aur detail mil jaye.

---

Dekho — **OS command injection** ke zariye abhi tu system pe command chala raha hai, lekin direct `peter` ke account ka **password** ya SSH access tere paas nahi aaya. Tu bas web application ke zariye us user ke rights ke under command run kar raha hai.

---

### 2 possible cases:

1. **Agar `peter` ek normal user hai**

   * Tere paas limited access hoga (sirf wahi files aur processes jo `peter` ko allowed hain).
   * Next step hamesha **Privilege Escalation** hota hai (jaise misconfigurations, SUID binaries, kernel exploits, etc. dekhna).

2. **Agar `peter` root/admin nikla**

   * Tere commands **full server control** ke equal ho jaati hain.
   * Tu files delete, modify, system crash, aur pura environment le sakta hai.

---

### Samajhne wali baat:

* Jo `whoami` ne output diya, usse bas yeh clear hota hai ke **application kis OS user ke naam se chal rahi hai**.
* Iska matlab yeh nahi ke tu abhi directly `peter` ka account hijack kar gaya.
* Tu abhi **web shell ke through restricted access** use kar raha hai.

---

💡 Real world main testers `whoami` ke baad hamesha commands try karte hain jaise:

* `id` → dekho kaunse groups main user hai.
* `pwd` → current directory check karo.
* `ls -la` → files dekh lo jo accessible hain.

---

### Kyun blacklist fail ho jati hai?

Blacklist ka matlab:
👉 Developer sochta hai ke dangerous characters block kar do (`;`, `|`, `&` etc.)
👉 Lekin attacker hamesha koi na koi **bypass trick** nikal leta hai.

* Encoding (`%0a`, `%26`)
* Alternative operators
* Different shell features

Example: Agar `;` block kar diya, attacker `|` ya `&&` try karega.

---

### Whitelist ka matlab:

👉 Sirf wohi input allow karo jo valid ho.

* Agar storeID number hona chahiye → sirf digits `0-9` allow karo.
* Agar filename hona chahiye → sirf safe pattern allow karo (`a-zA-Z0-9._-`).
* Baaki sab reject.

---

### Best practice developers ke liye:

1. **Direct shell call avoid karo** (commands ko code ke andar safe tarike se handle karo).
2. **Whitelist input validation** use karo.
3. **Parameterized APIs** ka use karo (jaise database ke liye prepared statements hote hain, waisa hi OS commands avoid karke safe libraries use karna).

---

💡 Agar tu dekh le ke developer sirf blacklist use kar raha hai, to 80% chance hai ke bypass possible hai.

---

### 1. Agar **Blacklist** use ho rahi ho

* Tum kuch special characters daalo (jaise `;`, `|`, `&`)
* Agar error aaya ya sirf woh ek character block hua, lekin baaki chalta raha → iska matlab blacklist hai.
  Example:

  ```
  storeId=1;whoami   ❌ Blocked
  storeId=1|whoami   ✅ Worked
  ```

  ➝ Matlab developer ne sirf `;` ko block kiya, baaki nahi.

---

### 2. Agar **Whitelist** use ho rahi ho

* Tumhare input main sirf allowed format accept hoga (jaise numbers hi numbers).
* Agar tumne kuch aur daala (letters, symbols, operators) to woh **pura reject ho jaata hai** — even ek character bhi allow nahi karega.
  Example:

  ```
  storeId=1         ✅ Worked
  storeId=1a        ❌ Invalid
  storeId=1;whoami  ❌ Invalid
  ```

  ➝ Matlab sirf `0–9` ki whitelist lagi hai.

---

### Real Testing ka tareeqa:

1. Ek ek operator try karo (`;`, `&`, `|`, `||`, `&&`)
2. Encoding try karo (`%0a`, `%26`, etc.)
3. Dekho kya block hota hai aur kya nikal jaata hai.

* Agar kuch bypass ho gaya = **Blacklist**
* Agar har cheez reject ho gayi = **Whitelist**

---

💡 PenTesting Tip:
Blacklist wali sites 90% exploitable hoti hain.
Whitelist wali sites exploit karna almost impossible hota hai (jab tak coding mistake na ho).
