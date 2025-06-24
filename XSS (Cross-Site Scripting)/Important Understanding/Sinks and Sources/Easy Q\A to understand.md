Jab main Pakistan search krta hoon to location.search se data nikalta hai aur document.write(Pakistan) mein chala jata hai

---

**Q:** Agar "Pakistan" ke liye location.search use ho raha hai, to agar koi aur cheez jaisy "cat", "movie", ya "Islamabad" search karen to kya source badal jata hai?

✅ Short Answer:
Nahi bhai, source badalta nahi.
Input jo bhi ho — word, animal, movie — agar wo URL ke through jaa raha hai, to location.search hi source hota hai.

---

Agar developer ne input sanitize kar diya ho, toh woh dangerous characters ko safe bana deta hai.
Jaise < > " ' / etc.

🔐 Example of sanitized input:

Tum payload bhejte ho:

```?search=<script>alert(1)</script>```

Lekin page pe dikhai deta hai:

```&lt;script&gt;alert(1)&lt;/script&gt;```

Yeh characters encoded ho gaye:

< ➝ &lt;

> ➝ &gt;

➡️ Iska matlab: JavaScript nahi chalaygi
➡️ Matlab: Sanitization ho chuki hai ✅

--- 

### Final Easily Understand Answer

1️⃣ Agar koi search box ho website pe
➡️ Aur main usmein duniya ki koi bhi cheez likhoon (jaise "cat", "movie", "Pakistan")
➡️ Toh wo input URL mein **?search=something** ya #something ki form mein chala jata hai.

2️⃣ Agar website ka page JavaScript use kar raha ho
➡️ Toh wo URL se value uthata hai **location.search** ya **location.hash** ke zariye
➡️ Fir wo value kisi variable **(q)** mein store hoti hai
➡️ Aur wo value kisi sink mein ja sakti hai jaise:

**document.write(q);**
**innerHTML = q;**
**insertAdjacentHTML('beforeend', q);**

3️⃣ Agar wo input directly sink mein ja rahi ho bina sanitize kiye,
➡️ Toh us waqt mujhe XSS payload daal kar test karna chahiye

Jaise:

```?search="><script>alert(1)</script>```

Ya:

```?search=<img src=x onerror=alert(1)>```

### 🧪 Tu XSS ka test tab kare jab:
URL mein input ja raha ho (?search=abc)

Page pe JavaScript **location.search** ya hash use kar rahi ho

Aur wo input **document.write**, **innerHTML**, etc mein jata ho

➡️ Toh tu payload daal ke test kar sakta hai XSS ka. ✅



