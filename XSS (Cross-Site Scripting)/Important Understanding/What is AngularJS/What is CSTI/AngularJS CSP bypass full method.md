# AngularJS CSP Bypass — Full Guide

## 🔍 Introduction

Aaj hum ne **AngularJS CSP Bypass** pe deeply kaam kiya — theory, practical labs, real life examples, aur har ek function ko asaan zubaan mein samjha. Yeh note future mein revision ke liye tayar hai.

---

## 📌 What is AngularJS?

AngularJS ek JavaScript framework hai jo mostly front-end web applications mein use hota hai. Iska kaam hota hai HTML pages ko dynamic banana using expressions, data binding, etc.

### Real-life Example:

Socho ek school hai jahan ek **teacher (AngularJS)** hai jo students (HTML elements) ko bolta hai kya display karna hai, kya calculate karna hai. Student sirf wahi kaam karega jo teacher bolega.

---

## 🛡️ What is CSP (Content Security Policy)?

CSP browser ki taraf se ek security shield hota hai jo unauthorized scripts ko block karta hai. Yeh `alert(1)`, `eval`, `window` jese dangerous cheezon ko allow nahi karta jab tak explicitly permission na di gayi ho.

### Real-life Example:

Ek **headmaster (CSP)** school mein rule banata hai ke koi bacha directly unke office (window object) nahi jaa sakta bina permission ke.

---

## 🔒 What is AngularJS Sandbox?

AngularJS sandbox ek protection layer hota hai jo JavaScript ke dangerous parts ko template expressions mein run hone se rokti hai.

### Real-life Example:

Socho ek **class monitor (sandbox)** har student ki activity check karta hai, aur agar koi rule break kare, wo usse rokta hai.

---

## ⚔️ Goal:

CSP + Sandbox dono security dete hain. Humara target hai ke dono ko trick karke `alert(1)` chalayin bina unko directly mention kiye hue.

---

## 🧠 Key Concepts with Real Life Explanation

### 1. `$event.path`

Jab user kisi input pe focus karta hai, browser automatically ek array create karta hai jisme us action mein involve sab elements listed hote hain.

**Example:**

```html
<input autofocus ng-focus="$event.path | orderBy: '[].constructor.from([1], alert)'"></input>
```

#### Real-life Example:

Ek bacha bench pe baithta hai (focus hota hai). CSP poochta hai: "Kaun kaun is focus mein involve tha?" Usse milta hai:

```
[input, form, body, html, document, window]
```

Last mein **window** bhi aa gaya — bas humne directly uska naam nahi liya, toh CSP confuse ho gaya 😎

---

### 2. `orderBy:`

AngularJS ka filter system hai jo arrays ko sort karne ke liye use hota hai. Lekin agar hum payload iske through bhejein, toh woh expression evaluate kar deta hai.

#### Real-life Example:

Teacher bolta hai: "Bacho, apne roll numbers ko arrange karo (orderBy)". Tum chhup kar us list mein ek bomb bhi daal dete ho (alert), teacher ko pata nahi chalta.

---

### 3. `[].constructor.from([1], alert)`

Yeh indirectly `Array.from([1], alert)` banata hai. Iska matlab hai: ek array lo aur uske har element pe `alert()` chala do.

#### Real-life Example:

Tum teacher ko kehte ho: "Sir, sab students ko test ka result individually suna dein (alert on each item)". Teacher maan jaata hai, aur unknowingly alert chala dete ho.

---

## 🎯 Alternative Method: `map(alert)`

Ek chhoti aur quick trick hai:

```javascript
[1].map(alert)
```

Yeh har element pe alert chala deta hai bina `window.alert()` likhe.

#### Real-life Example:

Tum kehte ho: "Sir, har student se individually baat karni hai (map) — unko bolna hai 'Alert!'" CSP confuse ho gaya phir se.

---

## ✅ Target Site Pe Kaise Check Karein?

### 🔍 Step-by-step Checklist:

1. **Check AngularJS**:

   * DevTools mein dekho: `<script src="angular.js">`
   * Console mein run karo: `angular.version.full`
   * Version < 1.6 ho toh vulnerable ho sakti hai

2. **Check Input Field + Event**:

   * Dekho koi `ng-focus`, `ng-click`, `ng-model` laga hua hai kya

3. **Check CSP**:

   * Burp Suite ya browser DevTools mein headers dekho:

     ```
     Content-Security-Policy: script-src 'self';
     ```
   * Agar `'unsafe-inline'` missing hai toh CSP strict hai

4. **Payload Inject Karo**:

   ```html
   <input autofocus ng-focus="$event.path | orderBy: '[].constructor.from([1], alert)'"></input>
   ```

---

## 📦 Final Important Summary (As Promised)

Jab hum kisi website ka input field dekhte hain jo AngularJS use karta ho aur us pe CSP (Content Security Policy) lagi ho, toh hum direct window object ya alert(1) nahi chala sakte, kyunki CSP in cheezon ko block kar deta hai. Lekin agar woh input field kisi event jese focus ko handle karta ho, toh hum us event ka use karke \$event.path access kar sakte hain — jo ek array hota hai jisme input se le kar last mein window object tak saari chain hoti hai. Fir hum AngularJS ka orderBy: filter use karte hain jisse ek sorting operation simulate hoti hai. Is filter ke andar hum .constructor.from(\[1], alert) ka use karte hain — jo secretly ek alert(1) chala deta hai bina directly window\.alert() likhe. Is tareeqe se hum AngularJS sandbox aur CSP dono ko confuse karke XSS achieve kar lete hain.
