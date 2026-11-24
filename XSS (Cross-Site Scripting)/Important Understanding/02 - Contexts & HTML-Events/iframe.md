🔍 Iframe kya hota hai?

👉 HTML ka ek tag hota hai ```<iframe>```

Jo kisi dusri website ya page ko tumhari website ke andar embed karta hai
Matlab ek page ke andar doosra page show karwa sakte ho.

---

🧪 Example:

```<iframe``` ```src="https://example.com"></iframe>```

Yeh kya karega?

Tumhari website ke andar example.com ka page open ho jaye ga — ek chhoti window mein.

---

🎯 JavaScript se iframe kaise banate hain?

```document.body.innerHTML = '<iframe``` ```src="https://example.com">';```

Ya

```let i =``` ```document.createElement("iframe");```
```i.src = "https://example.com";```
```document.body.appendChild(i);```

---

💥 Penetration Testing mein iframe ka use:

✅ 1. Clickjacking attacks:

Tum iframe mein kisi sensitive site ko load kar dete ho (jaise bank ka login page)

Uske upar invisible button rakh kar user se galat click karwa lete ho

✅ 2. Phishing pages embed karna:

iframe mein fake login page daal kar credentials churane ki koshish

✅ 3. Payload delivery:

Agar tumhare XSS payload mein iframe use hota hai, to tu malicious content silently load karwa sakta hai

Example:

```<iframe src="javascript:alert(1)"></iframe>```

---

❗Security mein iframe risky kyu hota hai?

iframe se attacker malicious site silently load karwa sakta hai

Agar XSS mil jaye, to attacker iframe ka use karke session hijacking, cookie stealing, ya drive-by attack kar sakta hai

---

### 🛡️ Protection against iframe attacks:

X-Frame-Options: DENY

Content-Security-Policy: frame-ancestors 'none'
Yeh headers use hote hain iframe ko block karne ke liye.

---

📘 Short Summary:

Iframe aik aisi window hoti hai jo dusri website ko current page ke andar load kar leti hai. JavaScript se agar attacker iframe daal de kisi trusted page mein, to wo user ko fool kar ke unka data le sakta hai — isi liye yeh penetration testing mein ek dangerous tool ban jata hai.