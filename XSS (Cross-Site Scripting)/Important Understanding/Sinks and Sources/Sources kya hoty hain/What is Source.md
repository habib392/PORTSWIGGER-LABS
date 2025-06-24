# Source hota kya hai?
🔸 Source = wo jagah jahan se attacker ki input JavaScript code tak pohanchti hai.
Yani:

- User ne kuch diya (URL, referrer, etc)
- Wo value JavaScript ke andar aa gayi
- Agar woh value bina check kiye kisi vulnerable sink (document.write, innerHTML, etc) mein chali gayi
- Toh ban gaya DOM XSS

### 🛰️ Top Sources in DOM XSS (Samajhne layak tareeqay se)

🔹 1. location.search

👉 Matlab: URL ka query string part

Example URL:

**https://site.com/page?search=Habib**

**let input = location.search; // input = "?search=Habib"**

Ya agar parsed form:

**let q = new URLSearchParams(location.search).get('search'); // q = "Habib"**

### 📌 Penetration Testing Tip:
Agar tum payload daal do:

```?search="><svg onload=alert(1)>```

Aur wo JS ke through document.write(q) mein chala gaya → XSS!

🔹 2. location.hash

👉 Matlab: URL ka hash/# wala part

Example URL:

```https://site.com/page#profile```

```let frag = location.hash; // "#profile"```
📌 Yeh source mostly single-page apps (SPA) mein use hoti hai.

Payload Example:

```#"><script>alert(1)</script>```

🔹 3. document.referrer
👉 Matlab: User kis page se aaya hai

let ref = document.referrer;
Example:

Tum attacker.com se redirect hue victim.com pe

Victim ka JS code document.write(document.referrer) kare

Toh tum attacker.com pe kuch malicious query string laga do:

```https://attacker.com/?evil=<script>alert(1)</script>```
📌 Agar victim ne referrer use kiya page pe → XSS ho sakta hai.

🔹 4. window.name
👉 Browser window ka ek special variable hai jo page switch hone par bhi same rehta hai.

let data = window.name;
Real-world use: Cross-page message passing

📌 Payload:

```window.name = "<script>alert(1)</script>"```
Agar agla page document.write(window.name) kare → boom

🔹 5. localStorage / sessionStorage
👉 Browser ki memory hai. JS access karti hai:

```let value = localStorage.getItem("token");```
📌 Agar koi input localStorage mein save hua aur wo directly HTML mein inject ho gaya → XSS

🔹 6. Cookies (via JS)
Cookies normally HTTP level pe hoti hain, lekin JS bhi read kar sakti hai:

```let c = document.cookie;```

📌 Agar tum JS code mein innerHTML = document.cookie dekh lo → 🧨

💥 Real-World Analogy

Socho ek news website hai:

```https://news.com/?search=Pakistan```
Woh tumhara search JS ke zariye page pe dikhata hai:

```let q = new URLSearchParams(location.search).get('search'); document.write("<p>Search Results for: " + q + "</p>");```

Tum yeh daal do:

```?search="><script>alert(1)</script>```
Toh q ban gaya attacker ka payload ⇒ inject ho gaya page mein ⇒ ho gaya XSS
