# 🛡️ XSS Lab Note — Reflected XSS with Event Handlers and `href` Blocked

## 🧪 Lab Overview:

This lab contains a **reflected XSS vulnerability**. Events like `onclick`, and attributes like `href`, `javascript:` are **blocked**. But some **SVG-related tags** are allowed.

> Goal: Inject a payload that says **Click**, and when clicked by the user, it should trigger `alert(document.cookie)`.

---

## ✅ Final Working Payload:

```html
<svg>
  <a xlink:href="data:text/html,<script>alert(document.cookie)</script>">Click</a>
</svg>
```

---

## 🔍 Key Concepts Explained (One by One):

### ✅ Q1: `xlink:href` kya hota hai?

**xlink\:href** SVG tags ke andar ek attribute hota hai jo kisi external resource (jaise link) ko point karta hai. Jab HTML mein normal `href` block ho, toh `xlink:href` ka use ek backdoor ki tarah hota hai XSS inject karne ke liye.

### ✅ Q2: `<svg>` ka kya role hai?

Normally `<svg>` web page mein vector graphics (shapes, icons, circles, etc.) banane ke liye use hota hai. Lekin XSS mein ye tag useful hota hai kyunki iske andar bohot se attributes allowed hote hain jab baaqi HTML filter ho jata hai.

> Jaise: `<svg><circle></circle></svg>` ek shape banata hai, lekin `<svg><a xlink:href=...>` payload run karta hai.

### ✅ Q3: `data:text/html,<script>...` ka kya matlab hai?

Ye ek special **URI scheme** hai jiska matlab hai:

* `data:` → hum kisi external file ka link nahi de rahe
* `text/html` → content HTML hai
* `<script>...</script>` → ye HTML code run hona chahiye jab user is link pe click kare

**Simple words:** Jab koi is link pe click karega, toh browser us `data:` URI ko as HTML interpret karega aur `<script>` execute karega.

### ✅ Q4: URI kya hota hai?

URI = Uniform Resource Identifier
Ye koi bhi string hoti hai jo kisi resource ko describe karti hai — jaise `http://`, `mailto:`, `ftp://`, `data:`, etc.

> `data:text/html,<script>alert(1)</script>` bhi ek URI hai jo browser ko batata hai ke HTML code chalao.

### ✅ Q5: `<svg>` hum ny khud lagaya ya already input mein tha?

Humne `<svg>` **khud lagaya** kyunki normal `<a href="">` block ho gaya tha. Isliye humne payload ko `<svg>` ke andar wrap kiya jisme events block nahi the.

### ✅ Q6: `data:` ka use kyu kiya jab `href` blocked tha?

Normally hum `<a href="https://example.com">Click</a>` likhte hain. Lekin yahan `href` block tha.

Isliye `xlink:href="data:text/html,<script>alert(1)</script>"` use kiya — taake:

* koi external site pe na jaye
* balki directly HTML code render ho aur XSS execute ho jaye

---

## 🎯 Real Life Attack Example (Step-by-Step)

### 🔹 Attacker ka kaam:

1. Vulnerable website pe input field test karta hai (search/comment/etc.)
2. Dekhta hai `href`, `onclick`, `javascript:` block hain ya nahi
3. Agar block hain, toh alternate payloads try karta hai — jaise `svg`, `xlink:href`, `data:` URI
4. Working payload nikalta hai:

   ```html
   <svg><a xlink:href="data:text/html,<script>alert(document.cookie)</script>">Click</a></svg>
   ```
5. Is payload ka full URL banata hai aur victim ko WhatsApp, email ya DM mein bhejta hai

### 🔹 Victim ka action:

1. Link open karta hai
2. Usse "Click" dikhai deta hai — curiosity se click karta hai
3. XSS execute hota hai → `alert(document.cookie)` chalta hai (ya silently cookie leak ho jati hai)

### 🔹 Attacker ko kya milta hai:

* Agar user logged-in tha → session cookie mil gayi → attacker hijack kar leta hai
* Agar user logged-out tha → attacker phishing ya keylogging kar sakta hai

---

## 🧠 Summary

| Item                 | Explanation                                                   |
| -------------------- | ------------------------------------------------------------- |
| `xlink:href`         | SVG mein link dene ka alternate tareeqa jab `href` blocked ho |
| `<svg>`              | Safe-looking tag jisme XSS sneaky tareeqe se inject hota hai  |
| `data:text/html,...` | JavaScript execute karwane ka URI trick                       |
| Victim action        | Link click + button click → payload execute                   |
| Attacker gain        | Session cookie, phishing, or keylogging                       |

---

## ✅ Final Baat:

> Jab HTML filters strict hoon, lekin `<svg>` aur `xlink:href` allow ho — toh ye technique ek powerful reflected XSS ban sakti hai. Bina events aur href ke bhi, attacker victim ko trap kar sakta hai ek simple "Click" ke zariye.
