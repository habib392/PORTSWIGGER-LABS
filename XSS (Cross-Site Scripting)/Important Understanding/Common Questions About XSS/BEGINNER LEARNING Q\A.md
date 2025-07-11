# MUST LEARN BEFORE XSS

### 🔹 Step 1: HTML ke Tags ka Role samajh

Tag	Kaam	Penetration Testing mein Kya Faida?

| Tag             | Kaam                      | Penetration Testing mein Kya Faida?                      |
| --------------- | ------------------------- | -------------------------------------------------------- |
| `<a>`           | Link banata hai           | `href=` mein `javascript:` inject ho sakta hai           |
| `<img>`         | Image show karta hai      | `onerror=` ka use karke XSS chalayenge                   |
| `<script>`      | JavaScript likhne ke liye | Direct XSS trigger karta hai                             |
| `<input>`       | User input field          | Reflected XSS test point hota hai                        |
| `<div>` / `<p>` | Content show karta hai    | Agar innerHTML use ho raha ho to vulnerable ho sakta hai |

---

### Step 2: Browser ke "Inspect Element" ka role

Tu jab kisi website pe inspect open karta hai (Right-click → Inspect)

Tu dekh sakta hai:

- Kya HTML structure hai?
- Kaun sa input kahan jaa raha hai?
- Input page pe reflect ho raha hai ya nahi?
- Kis tag ke andar aa raha hai?

Yahi se tu context samjhta hai!

---

### 🔹 Step 3: JavaScript ka XSS se kya connection?

XSS ka full form: **Cross-Site Scripting**

Matlab hum JavaScript ka code kisi aur user ke browser mein chala dete hain

Yeh kaam sirf isliye hota hai kyunki:

Developer ne user input ko bina escape kiye page mein daal diya
Browser ne us input ko text ki jagah code samajh liya

Example:

```Hello <script>alert('XSS')</script>```

---

### 🔹 Step 4: Payload daalne ka logic (Context Based)

| Reflection kahan ho rahi hai?        | Kaunsa Payload Try Karna Chahiye?          |
| ------------------------------------ | ------------------------------------------ |
| `<div>input_here</div>`              | `<script>alert(1)</script>`                |
| `<img src="input_here">`             | `x" onerror="alert(1)`                     |
| `<a href="input_here">`              | `javascript:alert(1)`                      |
| `var data = "input_here"`            | `";alert(1);//`                            |
| URL ke end mein `?search=input_here` | `<svg/onload=alert(1)>` ya encoded payload |

---

### Step 5: Agar reflection nahi nazar aa rahi?

Try encoding payload (%3Cscript%3Ealert(1)%3C/script%3E)

Try BurpSuite se decode kar ke dekh

Source code view karo (Ctrl+U)

Burp repeater mein test karo

