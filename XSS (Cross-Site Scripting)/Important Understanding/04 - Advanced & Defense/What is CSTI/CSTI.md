## 🧠 **Client-side Template Injection (CSTI) — Simple Explanation in My Words**

---

### 🔗 Useful Links:

* [Web Security Academy](https://portswigger.net/web-security)
* [Cross-site scripting (XSS)](https://portswigger.net/web-security/cross-site-scripting)
* [XSS Contexts](https://portswigger.net/web-security/cross-site-scripting/contexts)
* [Client-side Template Injection](https://portswigger.net/web-security/cross-site-scripting/contexts/client-side-template-injection)

---

## 📌 **Client-Side Template Injection (CSTI) kya hoti hai?**

CSTI ek aisi vulnerability hoti hai jo un websites mein milti hai jo **AngularJS jese template engines** ka use karti hain.

Jab koi website user input ko directly template ke andar daalti hai aur usse render karti hai, toh hum apna malicious input daal kar XSS execute kar sakte hain.

➡️ Is attack ko **PortSwigger ke researchers** ne pehli dafa popular banaya tha.
➡️ Ismein hum quotes (`'` ya `"`) ke bina bhi JavaScript code chala sakte hain.

---

## 🧰 **AngularJS Sandbox kya hoti hai?**

AngularJS ke andar ek **"sandbox"** hoti hai — jo ek **security wall** hoti hai. Yeh ensure karti hai ke koi attacker dangerous cheezein na chala sake jaise:

* `window`
* `document`
* `__proto__`
* `Function()`
* `call()`, `apply()`, `bind()`, etc.

Lekin baad mein researchers ne isko **multiple tareeqon se bypass** karna seekh liya tha.
Is wajah se AngularJS ne version **1.6 ke baad** ye sandbox remove kar di.

**Lekin ab bhi kai websites purane version pe chal rahi hoti hain — aur woh vulnerable hoti hain!**

---

## ⚙️ **Sandbox kaam kaise karti hai?**

Jab AngularJS kisi template expression ko evaluate karta hai:

1. Pehle wo JavaScript ko parse karta hai.
2. Phir usko rewrite karta hai (jese filters, safety checks).
3. Us rewritten code ko kuch functions se check karta hai, jese:

* `ensureSafeObject()` → dekhta hai ke koi self-referencing object toh nahi (e.g., `window`)
* `ensureSafeFunction()` → dekhta hai `call()`, `bind()` ya `constructor()` use toh nahi ho raha
* `ensureSafeMemberName()` → dangerous properties jese `__proto__` ya `__lookupGetter__` toh nahi use ho rahi

Tum isko **live dekh sakte ho** yahaan:
👉 [AngularJS Sandbox Fiddle](http://jsfiddle.net/2zs2yv7o/1/)

---

## 🚪 **Sandbox Escape ka kya matlab hai?**

Sandbox escape ka matlab hota hai **sandbox ko confuse karna ya uske guard functions ko disable karna.**

🔓 Example:

```js
'a'.constructor.prototype.charAt = [].join;
```

➡️ Is line mein `charAt()` function ko tod diya gaya hai
➡️ AngularJS ko lagta hai ke har character ek valid identifier hai
➡️ Is wajah se malicious payload jaise `"x=alert(1)"` allowed ho jaata hai

---

### 🔍 **isIdent Function kya hai?**

AngularJS ka internal function hota hai `isIdent()` jo check karta hai:

```js
isIdent = function(ch) {
  return ('a' <= ch && ch <= 'z' || 'A' <= ch && ch <= 'Z' || '_' === ch || ch === '$');
}
```

Lekin agar tum `charAt()` ko tod do, toh `isIdent('x9=9a9l9e9r9t9(919)')` return karega `true` — because ab wo har part ko "character" samajh raha hota hai.

---

## 🔨 **Advanced AngularJS Escape Without Quotes**

Kabhi kabhi websites quotes (`'` `"`) allow nahi karti.
Toh us case mein tumhe ye karna padta hai:

```js
String.fromCharCode(120,61,97,108,101,114,116,40,49,41)
```

➡️ Yeh `x=alert(1)` ko banata hai bina quotes ke
➡️ Lekin AngularJS normally `String()` ko allow nahi karta expression mein

Toh hum yeh trick use karte hain:

```js
toString().constructor.fromCharCode(...)
```

➡️ `"1"` ki string lete hain, uska `constructor` use karke `String` class access kar lete hain
➡️ Phir usmein `.fromCharCode()` use kar ke payload banate hain

---

### 🧪 `$eval()` function ki jagah `orderBy` filter ka use

Normal escape mein hum `$eval('x=alert(1)')` likhte hain.
Lekin lab mein `$eval` available nahi hoti.

Toh hum AngularJS ka **filter** use karte hain — jese `orderBy`.

Example:

```js
[1] | orderBy: <payload>
```

Yeh payload execute hota hai filter ke andar.

> **Note:** AngularJS mein `|` ka matlab JavaScript jesa bitwise OR nahi, yeh filter apply karne ke liye hota hai.

---

## 🔚 **Summary:**

* CSTI ka matlab hai: input expression ke form mein inject karna
* AngularJS sandbox XSS se bachaata hai, lekin bypass ho sakta hai
* `charAt = [].join` se sandbox confuse ho jaata hai
* Quotes blocked ho toh `fromCharCode()` use karo
* AngularJS ka `orderBy` filter bhi XSS execution ke liye use ho sakta hai

---

