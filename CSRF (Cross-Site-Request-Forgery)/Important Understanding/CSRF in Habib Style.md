## ✅ **CSRF Vulnerability Ko Pehchan’ne ke Habib ke Steps**

---

### 🔍 1. **Check karo kya CSRF token exist karta hai ya nahi**

* Burp Repeater mein form dekho (POST ya GET)
* Agar **CSRF token** nahi hai → 🔥 **vulnerability possible**

---

### 🧪 2. **Agar token hai, usay tamper karo**

* Token ko change karo (e.g. `abc123` → `xyz456`)
* Agar **response 302 ya 200 success** deta hai → 🔥 **vulnerability confirmed**

---

### 🔎 3. **Token hata do aur test karo**

* Pura CSRF token parameter hi hata do
* Agar phir bhi server action accept kar leta hai → ✅ **Confirmed CSRF**

---

### ⚖️ 4. **Token bind kis cheez se hai — user/session ya nahi?**

* Test karo: CSRF token User A ka hai
* Agar woh token User B ke session mein bhi kaam karta hai
  → 🔥 **Server validate nahi kar raha** — **vulnerability confirmed**

---

### 🧬 5. **Token ID aur Key same value accept karte hain ya nahi?**

* Dono fields mein `rock` daal do (jaise: `csrfID=rock` & `csrfKey=rock`)
* Agar server accept kar leta hai → 🤯 **weak token implementation**

---

### 🔁 6. **Request method check karo (POST → GET)**

* Agar original form POST hai
* Tum GET mein change kar do aur woh bhi accept ho jaye
  → ⚠️ **Bad CSRF protection — possible exploit**

---

## 🎯 Summary Line

"CSRF ka matlab sirf token hona nahi — server ka usay sahi tareeke se verify karna zaroori hai."
