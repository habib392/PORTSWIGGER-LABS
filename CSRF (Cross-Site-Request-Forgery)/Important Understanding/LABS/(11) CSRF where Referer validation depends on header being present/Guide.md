## 🔓 Referer-based CSRF Bypass

### 🔐 CSRF Protection aur Referer Header

Kuch websites CSRF attacks se bachne ke liye proper **CSRF tokens** use nahi karti, balkay woh **HTTP Referer header** check karti hain. Woh yeh dekhna chahti hain ke request **unki apni website se** aayi hai ya kahin aur se.

Agar Referer unki domain se ho, toh woh request allow kar deti hain. Agar kisi unknown site se ho (jaise `evil.com`), toh woh reject kar deti hain.

### 📩 Referer Header Kya Hota Hai?

Referer ek optional HTTP header hota hai jo browser **automatically bhejta hai**, jab user kisi link pe click karta hai ya form submit karta hai. Isme likha hota hai ke request **kahan se aayi hai** — yani pehla page ka URL.

Example:

```
Referer: https://example.com/form
```

### ❌ Isko Bypass Kaisy Karte Hain?

Yeh method secure nahi hoti kyunke attacker referer ko:

* **Hide** kar sakta hai
* **Modify** kar sakta hai

Ek simple tareeqa yeh hota hai:

```html
<meta name="referrer" content="never">
```

Is meta tag se browser referer header bhejna **band kar deta hai**.

Ab jab victim user form submit karta hai, toh target website ko referer milta hi nahi — aur agar website ne "referer ho tabhi check karo" jaisa logic lagaya ho, toh woh request **bina check ke allow** kar deti hai.

### ⚠️ Validation Agar Header Ho Tabhi

Kuch sites sirf tab referer check karti hain jab header present ho. Agar header hi na ho, toh woh validation **skip** kar deti hain — jo attacker ka moka hota hai.

### ✅ Pentester Kya Kare?

Agar tum dekhte ho ke site sirf referer check karti hai aur token nahi hai, toh tum exploit aise design karo jisme referer header **na ho** — jaise:

```html
<meta name="referrer" content="never">
<form action="https://target.com/transfer" method="POST">
  <input type="hidden" name="amount" value="1000">
  <input type="hidden" name="to" value="attacker">
  <input type="submit" value="Click Me">
</form>
```

### 🔐 Real Protection

Secure sites yeh karti hain:

* **CSRF tokens** ka use karti hain
* **SameSite cookies** ka use karti hain
* Referer check ko optional rakhti hain ya proper fail karti hain jab header missing ho

### 🧠 Lessons:

* Sirf Referer header pe depend karna **weak defense** hai
* Attacker referer ko **easily manipulate** kar sakta hai
* Secure sites ko **header missing hone par bhi request reject** karni chahiye
* Best practice: **CSRF token + SameSite cookie**

---

Tum is technique ko buray referer validation logic ka faida uthane ke liye pentest mein use kar sakte ho. Always test with and without Referer to see behavior.

