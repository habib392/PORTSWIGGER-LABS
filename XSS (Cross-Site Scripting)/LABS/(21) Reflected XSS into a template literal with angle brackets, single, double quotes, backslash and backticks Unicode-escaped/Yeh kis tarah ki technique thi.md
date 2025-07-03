# Questions/Answers 

### ✅ 1. Backtick bhi quotes ki tarah hota hai

Bilkul! Jaise:

'single quotes'

"double quotes"

`backtick quotes` ✅

Lekin backtick ka fayda zyada powerful hai — uske andar ${} daal ke JavaScript execute karwa sakte ho.

---

### ✅ 2. ${} JavaScript ka part hota hai

Jab browser dekhta hai:

```let a = `Hello ${1+2}`;  // becomes "Hello 3"```

Yani ${} ko wo JS code samajhta hai, string nahi

Toh agar tum input mein daal do:

${alert(1)}

Toh browser bolega:
"Arey yeh mera banda hai! Chalo alert(1) chala dete hain!" 😎

---

### 🧠 Real-World XSS Point:

Agar koi developer careless ho gaya aur:

let x = `${userInput}`;

Toh tum usi ${} ke through uska JavaScript tod sakte ho = Reflected XSS!

---

### 🔥 Summary in Teri Zubaan:

"Backtick bhi quotes jaisa hai, lekin uska andar ka JS browser khud hi chala deta hai agar tum usme ${} daal do — aur hum isi feature ko XSS ke liye use karte hain.”
