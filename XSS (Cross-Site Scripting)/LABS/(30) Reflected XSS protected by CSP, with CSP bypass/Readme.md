## 🔥 Lab: Reflected XSS with CSP Bypass

> 🎯 **Maqsad:** Aisi XSS injection karni hai jo CSP (Content Security Policy) ke baad bhi chale aur `alert(1)` pop-up aaye.
> ⚠️ **Note:** Yeh trick **sirf Chrome browser** mein kaam karti hai.

---

### 📌 Masla Kya Hai?

* Website pe XSS vulnerability hai (reflected XSS).
* Lekin site ne CSP lagayi hui hai jo `<script>` aur `<img onerror>` ko block karti hai.
* CSP ke andar `report-uri` ka path diya gaya hai jisme ek **`token`** naam ka parameter use ho raha hai.
* Yeh `token` tumhare control mein hai — matlab iske zariye hum CSP mein apna rule inject kar sakte hain.

---

### 🔍 Step-by-Step Hal

#### ✅ Step 1: XSS Test karo

Search box mein likho:

```html
<img src=1 onerror=alert(1)>
```

Result: Script reflect ho raha hai lekin run nahi ho raha — kyunki **CSP block kar rahi hai**.

---

#### 🔎 Step 2: Burp Suite se CSP check karo

Burp Proxy on karo aur request intercept karo.
Response headers mein yeh line milegi:

```
Content-Security-Policy: default-src 'self'; script-src 'self'; report-uri=/csp-report?token=XYZ
```

✅ Yeh `token=XYZ` tumhare control mein hai
✅ Yeh CSP ke andar use ho raha hai
✅ Iska matlab hai: **CSP injection possible hai!**

---

#### 🚀 Step 3: CSP Inject karo aur XSS chalao

Browser mein yeh URL open karo (lab ID apne hisaab se replace karo):

```
https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cscript%3Ealert(1)%3C%2Fscript%3E&token=;script-src-elem%20'unsafe-inline'
```

🧠 Breakdown:

* `search=` mein `<script>alert(1)</script>` diya gaya hai (URL encoded form mein)
* `token=` mein inject kiya gaya hai:

  ```
  ;script-src-elem 'unsafe-inline'
  ```

  * `;` pehle rule ko band karta hai
  * `script-src-elem 'unsafe-inline'` Chrome ko keh raha hai ke inline `<script>` allow kar do

---

### ✅ Natija

* CSP ke rules humne badal diye
* Script allow ho gayi
* Page load hote hi `alert(1)` ka pop-up aa gaya
* **Lab successfully solve ho gaya** ✅

---

### 🔐 Real-life Tip for Pentesters

> Jab bhi CSP lagi ho kisi site pe, yeh check karo ke koi parameter (jaise `token`, `ref`, `callback`) CSP ke andar use ho raha hai?
> Agar haan, to ho sakta hai tum us parameter ke through CSP modify kar sako.
> **CSP bypass karke XSS chala dena high severity vulnerability hoti hai.**
