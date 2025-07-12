## 🔐 SameSite Cookie Restrictions aur Bypass Techniques

---

### 🔸 What is SameSite Cookie?

SameSite ek browser security mechanism hai jo decide karta hai ke **cookie browser se kis request ke sath bheji jaye ya na bheji jaye**, jab woh request kisi doosri website (cross-site) se ho.

SameSite attribute cookies ko 3 tarah se control karta hai:

* `Strict` → Sirf apni hi website se request ho to cookie jayegi
* `Lax` → Kuch cross-site requests (GET method + user click) allowed
* `None` → Har request mein cookie jayegi (agar Secure attribute ho)

Yeh mechanism CSRF, cross-site leaks, aur kuch CORS attacks se partial protection deta hai.

---

### 🔸 Chrome ka Default Behavior

2021 se agar koi website SameSite attribute explicitly set nahi karti, to **Chrome `Lax` default mein laga deta hai**. Matlab:

> "Agar developer kuch na bhi set kare, to browser cookie ko limited tarah se hi send karega."

Yeh behavior **proposed standard** hai aur baaki browsers bhi gradually ise follow kar rahe hain.

---

### 🔸 What is "Site" in SameSite Context?

Site ka matlab hota hai: **TLD + 1 level domain**

Examples:

* `example.com` → site: `example.com`
* `app.example.com` → site: `example.com`
* `example.co.uk` → site: `example.co.uk` (yahan `.co.uk` is considered eTLD)

Even **scheme (http/https)** matter karta hai. Agar tum `http://example.com` se `https://example.com` ja rahe ho, to browser ise **cross-site** samjhta hai.

---

### 🔸 eTLD kya hota hai?

`eTLD` ka matlab hai **Effective Top-Level Domain**, jaise `.co.uk`, `.gov.pk`. Yeh multi-part domains hote hain jo ek unit ki tarah treat kiye jaate hain.

---

### 🔸 Site vs Origin — Farq samajhna zaroori hai:

| Term       | Scope / Meaning        |
| ---------- | ---------------------- |
| **Site**   | scheme + eTLD+1        |
| **Origin** | scheme + domain + port |

**Same-Origin hone ke liye:**

* Scheme same ho
* Domain exact same ho
* Port same ho

**Example Table:**

| Request from                                       | Request to                                                   | Same-site? | Same-origin?             |
| -------------------------------------------------- | ------------------------------------------------------------ | ---------- | ------------------------ |
| [https://example.com](https://example.com)         | [https://example.com](https://example.com)                   | ✅          | ✅                        |
| [https://app.example.com](https://app.example.com) | [https://intranet.example.com](https://intranet.example.com) | ✅          | ❌ (domain name alag hai) |
| [https://example.com](https://example.com)         | [https://example.com:8080](https://example.com:8080)         | ✅          | ❌ (port alag hai)        |
| [https://example.com](https://example.com)         | [https://example.co.uk](https://example.co.uk)               | ❌          | ❌                        |
| [https://example.com](https://example.com)         | [http://example.com](http://example.com)                     | ❌          | ❌ (scheme alag hai)      |

---

### 🔸 How SameSite Prevents CSRF:

CSRF attacks mein attacker kisi victim ke browser se target website ko **unwanted request bhejta hai**, usually through script/image/form, jismein victim ki authentication cookies lagti hain.

SameSite ensure karta hai ke:

* Aisi requests mein cookies include hoon ya nahi — based on attribute (`Strict`, `Lax`, `None`).

---

### 🔸 Set-Cookie Header Syntax:

```http
Set-Cookie: session=0F8tgdOhi9ynR1M9wa3ODa; SameSite=Strict
```

Agar `SameSite=None` use kar rahe ho to `Secure` attribute bhi hona **zaroori** hai:

```http
Set-Cookie: trackingId=0F8tgdOhi9ynR1M9wa3ODa; SameSite=None; Secure
```

---

### 🔸 SameSite Levels Explained:

#### ✅ Strict:

* Only sent in **same-site** requests
* Best for **sensitive actions** (e.g. login, payment)
* **Not sent** with any cross-site request (even via user click)

#### 🟡 Lax:

* Sent in **GET requests** with **top-level navigation**
* Not included in POST, iframe, script, or image loads
* Default behavior in Chrome if nothing is set

#### 🔴 None:

* Sent in **all requests**, including cross-site
* Must include **Secure** attribute
* Used in **3rd-party cookies** like ads, trackers

---

### ⚠️ Common Security Mistakes:

* Developer sets `SameSite=None` but forgets `Secure`
* Relies only on Lax without CSRF tokens
* Website breaks due to Chrome's Lax-by-default, so they disable SameSite completely — bad practice!

---

### 🧪 Bypassing SameSite Lax via GET (CSRF Attack)

Kuch servers GET aur POST mein fark nahi karte.
Agar SameSite=Lax hai, to tum **GET request se CSRF** karwa sakte ho agar victim link click kare ya browser redirect ho.

#### 🧨 Example (script based):

```html
<script>
  document.location = 'https://vulnerable-website.com/account/transfer-payment?recipient=hacker&amount=1000000';
</script>
```

#### 🧨 Example (form-based bypass using \_method):

```html
<form action="https://vulnerable-website.com/account/transfer-payment" method="POST">
  <input type="hidden" name="_method" value="GET">
  <input type="hidden" name="recipient" value="hacker">
  <input type="hidden" name="amount" value="1000000">
</form>
```

Frameworks jaise **Symfony** is `"_method"` parameter ko support karte hain. Isse POST request bhi **GET** jaisi treat ho sakti hai — cookie lag jaayegi.

---
