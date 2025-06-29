# Questions/Answers 

Kaisy pata chaly ga ky website kon sa tag allow kr rahi hai or kon sa block inspect tab main yeh kaisy show hoga agr main yeh payload daalon

```<img src=x onerror=alert(1)>```

### Answer

Jab tu koi XSS payload dalta hai, jaise:

```<img src=x onerror=alert(1)>```

toh yeh kaise pata chale ke website ne allow kiya hai ya block kar diya — iska tareeqa step-by-step yeh hai:

---

✅ 1. URL mein payload daal ke test kar

Example:

```https://example.com/search?q=<img src=x onerror=alert(1)>```

👀 Phir check karo:

🔹 A. Kya browser mein popup (alert) aaya?

Agar popup aya, matlab XSS success, tag aur attribute allowed the.

Agar nahi aya, toh block hua.

---

✅ 2. Inspect Element → Source dekho

Step-by-step:

1. Right click karo → Inspect

2. Elements tab open karo

3. Search box ka HTML locate karo jahan tumhara payload gaya tha
(Ctrl+F karo aur img likh ke search karo)

---

🔎 Ab kuch scenarios dekho:

🟥 Scenario 1:

```&lt;img src=x onerror=alert(1)&gt;```

Yeh encode ho gaya hai (```< → &lt;```)
👉 Matlab server ne sanitize kar diya = Blocked


---

🟨 Scenario 2:

```<img src="x">```

```onerror=alert(1)``` missing hai

👉 Matlab event attribute block kar diya = Half blocked

---

✅ Scenario 3:

```<img src="x" onerror="alert(1)">```

Yeh intact hai + alert chal gaya
👉 Fully allowed = XSS success 🎯

---

🧪 Extra Tip: Burp ka use karke aur achi tarah check kar

1. Request ko intercept karo

2. Burp Intruder mein payload daal ke different tags test karo (```<svg>, <math>, <img>, <script>```)


3. Response mein dekho kya tag server wapas bhej raha hai

---

### 🔧 Real Life Use:

WAF ya filter agar laga ho, wo <script> ya onerror block karega

Lekin onbegin, onload jaise uncommon attributes miss kar sakta hai

Isi liye penetration testers Burp Intruder se brute-force karke kaunsa tag/event allowed hai yeh test karte hain.

