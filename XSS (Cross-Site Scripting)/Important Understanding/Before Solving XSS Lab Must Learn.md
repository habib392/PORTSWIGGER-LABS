# 💡 Before Solving XSS 
Jab bhi XSS ka koi lab ya real website ka test karna ho, sirf `alert(1)` daal dena kaafi nahi hota. Pehlay yeh samajhna zaroori hai ke XSS **kaise** aur **kyun** chalti hai. Neeche sab kuch asaan zubaan mein likha gaya hai:

---

## ✅ 1. Input kahaan ja rahi hai (Reflection Context)

Sabse pehle yeh dekho tumhari input page ke kis hissa mein reflect ho rahi hai:

* HTML tag ke andar? `<div>INPUT</div>`
* Attribute ke andar? `<img src="INPUT">`
* JavaScript mein? `<script> var x = 'INPUT'; </script>`

Jis context mein tumhari input ja rahi hai, usi ke hisaab se payload banana hota hai.

---

## ✅ 2. Kaunse characters allowed ya blocked hain

Aksar sites kuch dangerous characters block kar deti hain jaise:

* `<`, `>`, `"`, `'`, `(`, `)`

Toh tumhe pata hona chahiye ke:

* Kaunse character block hain
* Aur unko bypass kaise kiya jaye (jaise `%3C` ya `/**/` se)

---

## ✅ 3. Kaunse events aur tags use ho saktay hain

Har baar `<script>` kaam nahi karta. Toh hum use karte hain:

**Events:**

* `onerror`, `onload`, `onclick`, `onmouseover`, `onfocus`, etc.

**Tags:**

* `<img>`, `<svg>`, `<iframe>`, `<body>`, `<math>`, `<object>`

Yeh cheezein help karti hain jab script tag block hota hai.

---

## ✅ 4. JavaScript ke kuch concepts zaroor samjho

Jaise:

* `throw` kya karta hai (error phenkta hai)
* `onerror` kya trigger karta hai
* `toString()` kaise hijack hota hai
* `x = x => {}` ka kya matlab hota hai (arrow function)

Yeh sab cheezein use hoti hain tricky XSS payloads banane mein.

---

## ✅ 5. Website ka structure observe karo

Sirf payload daalna kaafi nahi hota. Website ka code aur behavior samajhna padta hai:

* Kya JavaScript pehle se chal rahi hai?
* Kya koi function humari input ko manipulate karta hai?
* Kya koi button ya link press karne par XSS trigger hoti hai?

Structure samjho → Fir attack plan banao.

---

## ✅ 6. Filters aur WAF ko bypass karna seekho

Kahi dafa `alert`, `script`, `onerror` jese lafz block hote hain.
Tab use karo:

* Unicode (`\u0061lert`)
* Comment break (`onerror/**/=alert`)
* Expression chaining (`alert,1337`)

---

## ✅ 7. Alert() choti cheez hai — Samajh sabse badi cheez hai

XSS ka goal sirf `alert(1)` chalana nahi hota — us tak pohchnay ke liye tumhe:

* JavaScript ka structure samajhna padta hai
* DOM ka context dekhna padta hai
* Proper place pe inject karna padta hai

---

## 🧠 Final Note:

Agar tum yeh sab cheezein samajh gaye ho, toh samjho tum **XSS developer** ban gaye ho — jo har payload ko **samajh ke todta hai**, na ke bas copy paste karta hai 💥

---
