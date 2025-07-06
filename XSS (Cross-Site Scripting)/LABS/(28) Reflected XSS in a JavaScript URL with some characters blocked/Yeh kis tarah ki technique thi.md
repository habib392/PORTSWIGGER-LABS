# 🔥 Reflected XSS in JavaScript URL

## 🎯 Lab ka Maqsad:

Aisi reflected XSS vulnerability ko exploit karna hai jahan kuch characters (jaise `(`, `)` aur space) blocked hain. Humein `alert(1337)` chalana hai, lekin thoda chakkar laga ke.

---

## 🧪 Payload:

```js
'},x=x=>{throw/**/onerror=alert,1337},toString=x,window+'',{x:'
```

Yeh payload `postId` parameter mein daalna hota hai URL mein.

---

## 🏫 Real-Life School Wali Kahani (JavaScript School)

Har ek JavaScript command ko school ke student/teacher ki tarah socho:

---

### 🔹 `'},`

Ye aise hai jaise pehla lecture band kar rahe ho taake naya topic shuru ho.

📘 "Sir ne pehla chapter band kiya, ab main bolta hoon apna point."

---

### 🔹 `x = x => { throw onerror = alert,1337 }`

Ek naya student `x` aaya hai. Jab usse bulaoge (call karoge), woh kya karega? **Pathar pheke ga (throw)**

Aur pehlay se tay hai:

> "Jab pathar pheyka jaye, toh sir `onerror` alarm bajayein = `alert(1337)`"

📘 "Main jab boloon ga toh error phenk doon ga, aur alert baj jaye ga."

---

### 🔹 `toString = x`

JavaScript mein jab kisi object ko string mein convert karna ho, toh `toString()` method call hoti hai.

Hum keh rahay hain: "Agar koi toString() call kare, toh mera student `x` jawab de."

📘 "Principal ka number hattake naya number lagaya — ab call `x` uthaye ga."

---

### 🔹 `window + ''`

Yeh string conversion hai. Jab JavaScript yeh dekhti hai, woh automatically `window.toString()` chalati hai.

Aur kyunki humne `toString = x` set kiya hai, toh `x()` call hota hai — jo error pheknay wala function hai.

📘 "School ka naam string mein badlo → Principal call hoga → x uthaye ga → pathar pheka jaye ga → alert bajay ga."

---

## ⚙️ Extra Concepts:

### 🔹 **Expression Chaining:**

```js
let a = (1, 2, 3); // Sirf 3 return hoga
```

Comma se multiple expressions likh sakte ho, lekin sirf last value return hoti hai.

> Lab mein: `alert, 1337` likha gaya, lekin sirf `1337` throw hota hai.

---

### 🔹 **Interpretation:**

JavaScript **line by line** code ko read aur samajhti hai. Yeh code messy lagta hai, lekin JS usay theek se tod kar samajh leti hai.

---

## 🔁 Full Flow:

1. Purana JavaScript block band karo → `'},`
2. Naya function banao `x` → `x = x => { throw ... }`
3. `onerror = alert` set karo
4. `toString = x` karo (function override)
5. `window + ''` likho → JS `toString()` call kare gi
6. `x()` chale ga → `throw` hoga → `alert(1337)` chale ga ✅

---

## ✅ Meri Observation:

```
✅ 1. x aik aise x ke barabar hai jisme aik activity ho rahi hai (error phenki ja rahi hai)
✔️ Bilkul sahi!
Yani x = x => { throw ... } mein tum x naam ka student bana rahe ho jo pathar phenke ga (i.e., error throw kare ga) jab usse call kiya jaye.

---
✅ 2. throw us x ke andar ka action hai — wohi pathar phenk raha hai jo onerror ko activate karta hai
✔️ 100% correct!
throw ka kaam hai error intentionally create karna jisse onerror trigger hota hai.
Aur tumne onerror = alert set kiya hua hota hai — toh woh alert(1337) chala deta hai.

---
✅ 3. toString = x ka matlab string bananay wali function ko us student (x) se replace kar diya
✔️ Bilkul sahi samjha!
Tumne toString() method ko hijack kar liya — jab JS kahe "object ko string mein convert karo", toh woh x() run kare — jo ke error phenkta hai (aur XSS trigger karta hai).

---
✅ 4. Jab hum window + '' likhtay hain toh JS samajhti hai ke toString() call karna chahiye
✔️ Exactly!
JavaScript internally bolegi:

> "Window ko string banana hai? Chalo window.toString() call karo."

Lekin tumne toString = x set kiya hai
Aur x() woh function hai jo XSS chala deta hai 🔥

---
🏁 Final Verdict:

✅ Tumne XSS ka full flow perfectly samjha:
- Break existing JS
- Inject function
- Set error
- Hijack toString
- Trigger via coercion (window + '')
- Result: alert(1337) fire = XSS ✅
```
