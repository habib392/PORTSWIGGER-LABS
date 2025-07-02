# 💡 Lab ka Naam
Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped

## 🔧 Step-by-step Solution (A to Z):
### ✅ Step 1: Random comment submit kar
Lab open kar.

Comment post kar — Name, Email, Website, aur Comment fields bhar.

Website field mein koi random string daal, jaise xyz123 — isko modify karna hai baad mein.

---

### ✅ Step 2: Burp Suite mein intercept karo
Intercept ON karo.

Jab tu comment submit karega, Burp us request ko pakar lega.

Us request ko "Send to Repeater" kar de.

Ab forward kar de ya browser se manually page refresh kar le.

---

### ✅ Step 3: Page load hone par Burp se check kar
Page dekhne ke liye jab browser request karega (e.g. blog post open karega), wo request bhi Burp pakdega.

Is second request ko bhi "Send to Repeater" kar.

Repeater mein jaa aur us request ka response section dekh.

---

### ✅ Step 4: Reflection check kar
Dekh ke tujh ka xyz123 (ya jo random string daala tha Website field mein) onclick="..." ke andar appear ho raha hai.

Aur HTML encoding kuch aisi ho rahi hogi:

```onclick="return window.open('xyz123')"```

### 🔒 Note:

- " " (double quotes) HTML encoded hain &quot;

- ' (single quote) escaped ho rahi hai \'

- \ (backslash) bhi escaped ho rahi hai

---

### 🎯 Goal: Inject JS onclick ke andar jo execute ho jab naam pe click ho
#### ✅ Step 5: Payload inject kar
Wapas comment form open kar.

Website field mein yeh payload daal:

```http://x.com?&apos;-alert(1)-&apos;```

🔍 Breakdown:

```&apos;``` => ' banega jab render hoga

Tera payload ban jayega:

```tracker.track('http://x.com?'-alert(1)-'');```

Yeh break karega JS string ko aur tera alert(1) execute hoga.

---

### ✅ Step 6: Test karo
Page reload kar.

- Apna comment dhoondh.
- Apne naam pe click karo.
- Agar sab kuch sahi gaya, to alert(1) aayega ✅

### 📌 Penetration Testing Tips:
Ye technique Stored XSS hai jo real-world mein comments, user profiles, ya admin panels mein mil sakti hai.

Is case mein attacker user ke naam ya profile link mein XSS inject karta hai.

Use Burp to check where reflection ho rahi hai, aur encoding kis tarah ho rahi hai.

Jab bhi ", ', ya \ escape ho rahi ho, toh alternate breaking technique use kar — jaise yahan &apos; use kiya gaya.
