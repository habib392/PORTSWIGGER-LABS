# Reflected XSS in canonical link tag

### 🔍 Lab ka Summary:

Reflection ho rahi hai ```<link rel="canonical" href="...">``` mein

Angle brackets ```(< >)``` escape ho rahe hain, matlab ```<script>``` work nahi karega.

But: aap attributes inject kar sakte ho jaise: accesskey, onclick, etc.

Target: Aisi attribute injection karo jisse alert(1) call ho jaaye jab banda key press kare.


### ✅ Step-by-Step Solution:

🧠 Step 1: Understand Reflection
Jab aap koi query string bhejtay ho like:

```https://YOUR-LAB-ID.web-security-academy.net/?test=abc```

toh abc reflect ho raha hai canonical link ke href attribute mein, like:

```<link rel="canonical" href="?test=abc">```

---

🎯 Step 2: Payload Injection

Hum ```< ya >``` use nahi kar sakte, magar hum " (double quote) close kar ke naya attribute inject kar sakte hain.

✅ Final Payload:

```?"accesskey='x'onclick='alert(1)```

Full URL banega:

```https://YOUR-LAB-ID.web-security-academy.net/?"accesskey='x'onclick='alert(1)```

🧠 Yeh kya karega?

Isse ```<link>``` tag tootega, aur attribute inject hoga poori ```<body>``` tag ya ```<html>``` tag pe (depending on how Chrome handles it).

🧪 Step 3: Trigger Exploit
Jab koi ALT+SHIFT+X press karega (Windows), to browser accesskey="x" ki wajah se focus ya click karega, aur onclick='alert(1)' chalega.

✅ Final Steps:
Open this URL:

```https://YOUR-LAB-ID.web-security-academy.net/?"accesskey='x'onclick='alert(1)```

Page load hone ke baad, press:

- Windows: ALT + SHIFT + X
- Linux: ALT + X
- Mac: CTRL + ALT + X

✅ alert(1) aajayega — Lab Solved!

---

### 💡 Penetration Testing Tip:
Is technique ko HTML Attribute Injection kehte hain. Real life mein agar ```<link>, <meta>,``` ya koi bhi tag
user input accept kar raha ho, toh yeh ek attack surface hoti hai — jo reflected
XSS ke liye kaam aa sakti hai. Accesskey + onclick ek smart combo hai jab script tag block ho.
