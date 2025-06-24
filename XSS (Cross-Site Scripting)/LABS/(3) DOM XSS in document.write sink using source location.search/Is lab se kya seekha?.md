# Is lab se kya seekha?
🔸 Lab: DOM XSS in document.write sink using source location.search

✅ Seekhne wali 5 core baatein:

Point	Samajh

1️⃣	Agar user input (location.search) directly document.write() mein jaye, to DOM-based XSS ka chance hota hai.

2️⃣	document.write() bina sanitize kiye input ko inject karta hai, isliye dangerous hai.

3️⃣	Tumne img tag ke andar input gaya hua dekha — usse break karke tag se bahar nikalna aur apna payload inject karna real skill hai.

4️⃣	Yeh vulnerability client-side JavaScript mein hoti hai, server-side kuch nahi karta.

5️⃣	Aisi vulnerabilities automated scanners detect nahi karte — isliye manual testing aur observation skills zaroori hain.

---

⏰ Kis time kya daala gaya aur uska kya effect hua?         
**Step 1** main yeh daala gya **?search=test123** 

Pata chala Input HTML mein inject ho raha hai

**Setp 2** main yeh kiya View Source ya Inspect mein code dekha	

Confirm hua ke input **document.write()** mein ja raha hai

**Step 3** main yeh daala ```?search="><svg onload=alert(1)>```

Payload run hua = XSS pop-up

Input attribute se nikal gaya aur naya tag inject kar diya	

Yeh hai real DOM-based XSS ka essence

🎯 Technique ka naam:
✅ DOM-based Cross-Site Scripting (DOM XSS)
✅ Sink: document.write()
✅ Source: window.location.search

🧠 Isko Master Kaise Karo?

🔹 1. Source & Sink samajho

### Sinks jaise:

- document.write()
- innerHTML
- eval()
- setTimeout()
- insertAdjacentHTML

### Sources jaise:

- location.search
- location.hash
- document.referrer

➡️ In combinations ko pehchano — DOM XSS tab hoti hai jab source input controlled ho aur sink unsanitized ho.

