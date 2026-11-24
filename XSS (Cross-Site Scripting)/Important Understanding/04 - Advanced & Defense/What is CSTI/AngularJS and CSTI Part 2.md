# CSTI PART 2

---

### 🧠 **CSP (Content Security Policy) Bypass AngularJS Mein Kaise Kaam Karta Hai?**

AngularJS mein CSP bypass bhi almost **sandbox escape** jaisa hi hota hai, lekin ismein **thoda HTML injection** bhi hota hai.

#### 🔒 Jab CSP Active Ho:

* AngularJS **template expressions** ko thoda **alfa tareeqay se parse** karta hai.
* Wo **`Function()` constructor** ka use avoid karta hai.
* Is wajah se tumhara normal payload (`charAt`, `fromCharCode`, etc.) kaam nahi karega.

---

### 🎯 **Toh phir hum kya karte hain CSP bypass ke liye?**

AngularJS kuch **custom events** define karta hai, jismein ek special variable hota hai: **`$event`**

➡️ `$event` = browser ka event object
➡️ Isme ek property hoti hai **`path`** (sirf Chrome browser mein available)

```js
$event.path  →  [input, form, body, html, document, window]
```

➡️ List ka **last element hamesha `window` hota hai**, jisko hum use karte hain bypass ke liye.

---

### 🧪 **Live CSP Bypass Example:**

```html
<input autofocus ng-focus="$event.path|orderBy:'[].constructor.from([1],alert)'">
```

#### Iska Breakdown:

* `ng-focus` = jab input box pe focus aayega (auto ho raha hai `autofocus` se), toh payload run hoga.
* `$event.path` = event path array (Chrome specific), jiska last item `window` hai.
* `orderBy:` = AngularJS filter (payload execute karne ka tareeqa)
* `'[].constructor.from([1],alert)'` = hum `alert()` function ko chala rahe hain har array element pe

#### ⚠️ Kya special hai?

* **Hum `window.alert()` directly nahi likh rahe**, warna sandbox pakar lega
* `from()` function backend mein use karta hai `window` ko, **lekin directly nahi dikhata**
* Is technique se sandbox confuse hota hai, aur XSS ho jaata hai

➡️ PortSwigger ne isi concept pe based ek **56 characters ka CSP bypass payload** bhi banaya tha:
[AngularJS CSP Bypass in 56 characters](https://portswigger.net/research/angularjs-csp-bypass-in-56-characters)

---

### 🧠 **Advanced Sandbox Escape (length restriction wale lab ke liye)**

Agar tumhara payload character limit mein fit nahi ho raha, ya koi part block ho raha ho, toh tum **alternate tricks** use kar sakte ho:

#### ✅ Simple & Chhota Bypass:

```js
[1].map(alert)
```

➡️ `map()` function ek array ka method hota hai, jo har item pe tumhara function chala deta hai
➡️ **Yahan hum `alert()` function ko pass kar rahe hain** bina `window` likhe

**Is wajah se sandbox confuse ho jaata hai** — kyunki usko `window.alert` dikh hi nahi raha, sirf `alert` function dikh raha hai.

---

### 🛡️ **Client-side Template Injection (CSTI) se bachne ka tareeqa:**

#### ❌ Kya na karo:

* Kabhi bhi **user input ko directly templates mein** mat daalo
  (jese `{{userInput}}` ya `ng-model="userInput"`)

#### ✅ Kya karo:

* Agar zaroori ho toh:

  * Template syntax (`{{`, `}}`, `|`, etc.) ko **sanitize ya filter** karo
  * Sirf **trusted sources** ka hi input templates mein inject karo

#### ⚠️ HTML Encode karna kaafi nahi hota!

Frameworks (jese AngularJS) pehle HTML decode karte hain, uske baad template expressions evaluate karte hain — isliye encode hone ke bawajood XSS ho sakta hai.

---

## 🧠 Short Summary:

| Topic              | Simple Explanation                                           |
| ------------------ | ------------------------------------------------------------ |
| **CSP**            | Extra security layer to block inline JS and dangerous code   |
| **Sandbox**        | AngularJS ka guard system to stop XSS                        |
| **\$event.path**   | Chrome ka event object path, jisme `window` bhi hota hai     |
| **orderBy filter** | AngularJS ka expression evaluator (used for exploitation)    |
| **from()**         | `[].constructor.from()` se window access indirectly hota hai |
| **map(alert)**     | Short and clean way to run code without window reference     |
| **Prevent CSTI**   | Never render untrusted user input inside templates           |
