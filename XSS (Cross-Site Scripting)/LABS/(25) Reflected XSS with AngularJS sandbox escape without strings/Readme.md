## Lab 25: Reflected XSS with AngularJS sandbox escape without strings

---

### Goal:
AngularJS app ke andar sandbox escape karna hai — without using any strings like 'alert' ya "$eval"

Matlab:

' " eval()` sab blocked hai ❌

Tumhein pure AngularJS object traversal aur logic se kaam lena hai ✅

---

### 🧠 Concept Breakdown

AngularJS mein:

Normally sandbox hota hai jo dangerous code run hone se rokta hai

Yeh sandbox tumhein alert(1) jese code run nahi karne deta

But agar tum toString().constructor.fromCharCode(...) jese tareeqon se code likho, toh tum sandbox bypass kar sakte ho — bina quotes ke

---

Payload Explanation:

```1&toString().constructor.prototype.charAt=[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1```

---

### ✳️ Breakdown:
toString().constructor

Yeh deta hai String constructor

Equivalent of: "".constructor → String

toString().constructor.prototype.charAt = [].join

Hum charAt function ko overwrite kar dete hain

### Iska matlab: jab bhi string use hogi, woh ab Angular sandbox ko confuse karegi

[1] | orderBy: toString().constructor.fromCharCode(...)
AngularJS filter lagate hain (orderBy) aur usmein dangerous code inject kar dete hain

fromCharCode(...) se string banate hain without quotes

---

```fromCharCode(120,61,97,108,101,114,116,40,49,41)```

Yeh kya banata hai?

String.fromCharCode(120,61,97,108,101,114,116,40,49,41)

➡️ Output: "x=alert(1)"

Yeh payload x = alert(1) run kar deta hai
Jo ke ek JS statement hai — and browser pe alert pop karta hai ✅

---

### ✅ Final Working Payload (Already Given)
Replace YOUR-LAB-ID with your real one:

```https://YOUR-LAB-ID.web-security-academy.net/?search=1&toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1```

---

### Real World Penetration Testing Use:
Yeh technique tab useful hoti hai jab:

AngularJS ka version 1.6 se pehle ho (sandbox escape possible ho)

Developer ne filters ya expressions directly use kiye ho user input pe

Tumhe bypass karna ho quotes block ya CSP

Bug bounty mein agar tumhe AngularJS mile, toh yeh type ka attack check karna banta hai!

---

### 🎓 Summary:
| Part                     | Kya ho raha hai                                   |
| ------------------------ | ------------------------------------------------- |
| `toString().constructor` | String constructor mil raha hai                   |
| `.fromCharCode(...)`     | Bina quotes ke string banayi ja rahi hai          |
| `charAt = [].join`       | Sandbox confuse kiya ja raha hai                  |
| `alert(1)`               | Executed without quotes, thanks to sandbox escape |
