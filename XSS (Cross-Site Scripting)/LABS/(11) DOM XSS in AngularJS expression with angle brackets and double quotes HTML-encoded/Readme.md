# DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

### 🔍 Lab ka Setup 

- Tu jab kuch bhi search karta hai, woh text HTML source mein {{searchText}} jese AngularJS expression mein daala jaa raha hota hai.
- Page mein ng-app directive laga hota hai, matlab AngularJS enabled hai.
- Double quotes " " HTML encoded hain.

Example: " → ```&quot;```

### ❗ Important Point
AngularJS jo curly braces ({{ }}) ke andar JavaScript evaluate karta hai, usmein:

- Hum " " use kiye bina bhi kuch payload daal sakte hain.
- Kyun? Kyunki AngularJS ke expressions bina quotes ke bhi evaluate ho jaate hain.

### 🧠 Payload ka logic
Payload hai:

```{{$on.constructor('alert(1)')()}}```

Breakdown:

```{{$on}}``` → Angular ka internal function hota hai.

```.constructor('alert(1)')``` → JavaScript ka tarika hai string ko function banane ka.

() → Us function ko turant call kar diya.

Matlab: alert(1) fire ho jaayega bina " " use kiye hue ✅

---

### ✅ Lab Solve Karne Ka Tareeqa (Step-by-Step)
Kuch random search kar — jaise abc123.

Page source dekho (Ctrl+U) → Dekh yeh abc123 kahaan inject ho raha hai 

Ab yeh confirm ho gaya ke AngularJS expression as it is execute ho raha hai.

Ab search box mein payload daal:

```{{$on.constructor('alert(1)')()}}```

Search pe click kar — agar sab sahi hua to alert pop-up aa jaayega 😎

---

### 💡 Penetration Testing Tip
AngularJS ka misuse modern web apps mein kabhi kabhi hota hai jab:

Developer input directly {{ }} ke andar inject kar deta hai.

Input ko sanitize nahi karta.

Agar tu bug bounty ya real-world pentesting mein AngularJS app dekhe, toh try kar:

```{{constructor.constructor('alert(1)')()}}```

```{{[].__proto__.constructor('alert(1)')()}}```

Yeh cheeze DOM-based XSS find karne ke pro level payloads hain.
