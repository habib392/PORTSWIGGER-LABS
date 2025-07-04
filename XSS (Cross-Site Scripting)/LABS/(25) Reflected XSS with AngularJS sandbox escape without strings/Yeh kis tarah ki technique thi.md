# All Important About This Lab
---

### 🧾 **GitHub Note: Lab - Reflected XSS with AngularJS Sandbox Escape Without Strings**

---

### 🧠 **Lab Summary:**

**Lab Name:** Reflected XSS with AngularJS sandbox escape without strings
**Goal:** AngularJS ke sandbox ko bypass kar ke `alert(1)` run karna — bina single `'` ya double `"` quotes use kiye
**Payload:**

```js
1&toString().constructor.prototype.charAt=[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
```

---

### 📚 **Concepts Covered in This Lab:**

* AngularJS kya hota hai
* Sandbox kya hota hai
* Sandbox escape ka matlab kya hai
* `charAt` aur `fromCharCode` ka role
* Quotes bypass karnay ka tariqa
* AngularJS filter abuse
* Reflected vs DOM XSS
* AngularJS version check karnay ka method
* Browser console aur Burp decoder ka use

---

### 🔹 **AngularJS kya hota hai?**

AngularJS aik JavaScript framework hai jo web apps ko **dynamic** banata hai.
Yeh client-side code chalata hai aur expressions jese `{{2 + 2}}` ko run karta hai bina page refresh ke.

**Example:**

```html
<p>{{ 2 + 2 }}</p>  <!-- Output: 4 -->
```

AngularJS browser mein JavaScript code evaluate karta hai, isliye agar validation na ho toh XSS ke liye vulnerable ho sakta hai.

---

### 🔹 **AngularJS version kaise check karein?**

✅ Open browser → DevTools → Console → Type:

```js
angular.version.full
```

✅ Ya page source mein dekhein:

```html
<script src="/resources/angular-1.4.9.min.js"></script>
```

👉 **Sandbox escape mostly version 1.6 se pehle possible hota hai**

---

### 🔹 **Sandbox kya hota hai AngularJS mein?**

**Sandbox = protection layer**
AngularJS ke sandbox ka kaam hota hai dangerous expressions (jaise `alert`, `eval`) ko block karna.

Yeh check karta hai:

* Tumhara input safe hai ya nahi
* Koi dangerous function use toh nahi ho raha

**Lekin hum is lab mein sandbox ko confuse karte hain, taake wo guard hata diya jaye.**

---

### 🔹 **Sandbox escape ka matlab kya hota hai?**

Sandbox normally tumhara payload rokta hai.
**Escape ka matlab:** uske internal guard functions (jaise `charAt`, `constructor`, etc.) ko overwrite karke confuse karna.

---

### 🔹 **Payload Breakdown:**

#### ✅ Full payload:

```
1&toString().constructor.prototype.charAt=[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
```

---

### 🔎 **Line-by-Line Explanation:**

#### 1️⃣ `1&`

➡️ Sirf query parameter starter hai
📌 **No real purpose** (can ignore)

---

#### 2️⃣ `toString().constructor.prototype.charAt = [].join`

✔️ **Very important**

* `toString()` → returns `"1"`
* `.constructor` → gives us `String` class
* `.prototype` → gives us `String.prototype`
* `.charAt = [].join` → overwrites `charAt()` function with useless join function

➡️ Result: AngularJS sandbox confuse ho jaata hai
➡️ Iska kaam block karne wala guard hata diya jata hai ✅

---

#### 3️⃣ `[1]|orderBy:`

* `[1]` = ek array
* `|orderBy:` = AngularJS filter apply karna

➡️ **Filter abuse** hoti hai taake tumhara payload AngularJS evaluate kare
➡️ `orderBy:` ke andar tum expression inject karte ho

---

#### 4️⃣ `toString().constructor.fromCharCode(...)`

```js
toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)
```

➡️ yeh generate karta hai: `"x=alert(1)"`

| Number | Character |
| ------ | --------- |
| 120    | x         |
| 61     | =         |
| 97     | a         |
| 108    | l         |
| 101    | e         |
| 114    | r         |
| 116    | t         |
| 40     | (         |
| 49     | 1         |
| 41     | )         |

📌 **Reason:** Quotes (`'`, `"`) allowed nahi the, isliye `fromCharCode` se string banayi ✅

---

#### 5️⃣ `=1`

➡️ Yeh extra assignment hai jisse AngularJS ko lagta hai ke expression valid hai
➡️ Final expression ho jaata hai:

```js
x = alert(1)
```

Jisko AngularJS evaluate kar deta hai

---

### 💻 **charAt vs fromCharCode – Difference**

| Function            | Kaam                                                               |
| ------------------- | ------------------------------------------------------------------ |
| `charAt(index)`     | String ka ek letter return karta hai (`"abc".charAt(1)` → `"b"`)   |
| `fromCharCode(...)` | Number se character banata hai (`String.fromCharCode(97)` → `"a"`) |

---

### 🧪 **Decode karne ka tariqa (manual test):**

#### ✅ Browser Console:

```js
String.fromCharCode(120,61,97,108,101,114,116,40,49,41)
→ "x=alert(1)"
```

#### ✅ Burp Suite Decoder:

1. Decoder tab → paste karo: `120,61,97,108,...`
2. Click: "Decode as" → ASCII from Decimal

#### ✅ Online Tool:

* [https://www.unit-conversion.info/texttools/ascii/](https://www.unit-conversion.info/texttools/ascii/)

---

### 📌 **Kya yeh DOM XSS tha?**

**❌ Nahi!**

* Yeh **reflected AngularJS-based XSS** tha
* DOM XSS tab hoti hai jab `document.URL`, `location.hash` waghera se direct DOM access ho
* Yahan humara input **expression** ke andar evaluate ho raha tha (AngularJS style)

---

### ✅ **Important cheezein samajhne wali:**

| Part                | Purpose                                        |
| ------------------- | ---------------------------------------------- |
| `toString()`        | String return karne ke liye                    |
| `.constructor`      | String class access karna                      |
| `.prototype.charAt` | Sandbox ka guard function                      |
| `[].join`           | Useless function se charAt replace karna       |
| `orderBy:`          | AngularJS filter jisme payload inject hota hai |
| `fromCharCode(...)` | Quotes bypass kar ke payload banana            |
| `=1`                | Expression complete karna                      |
| `[1]`               | Filter ka input (filler)                       |

---

### ✅ **Final Payload Flow (Sequence):**

1. Start with `1&` → dummy
2. Disable sandbox by overwriting `charAt`
3. Inject filter using `orderBy`
4. Build payload with `fromCharCode`
5. Complete expression with `=1`
6. Browser executes: `x = alert(1)` → 💥

---

### 🎯 **Penetration Testing Tip (Real World)**

* Ye techniques useful hoti hain agar:

  * AngularJS version < 1.6 ho
  * Developer ne user input directly AngularJS expression mein daal diya ho
  * Quotes blocked hon
* Bug bounty mein AngularJS payload test karna banta hai!

---

### ✅ **Lab Successfully Complete If:**

* Tumne sandbox escape kiya
* Browser ne `alert(1)` run kiya
* Lab automatically solved message aaya

---

### 🏁 **Conclusion:**

> Yeh lab ek advanced reflected XSS attack ka perfect example tha, jisme humne:
>
> * Sandbox ko confuse kiya
> * Quotes ke bina string banayi
> * Filter ke zariye payload inject kiya
> * AngularJS ko exploit karke payload execute karwa liya

---

