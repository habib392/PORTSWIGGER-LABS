# 🏊‍♂️ Sink kya hota hai? (Basic Concept)

📌 Soch lo:

Sink = wo jagah jahan tumhara input finally use hota hai (inject hota hai)
Aur agar woh jagah dangerous ho aur input sanitize na ho, to attacker JavaScript chala sakta hai.
Matlab: wahan XSS ho sakta hai!


### 💡 DOM XSS ke Top 5 Dangerous Sinks:

Chalo ab har sink ko easy example ke saath samajhte hain 👇


1️⃣ document.write()

➡️ Sabse common aur sabse khatarnaak sink

let input = location.search;
document.write(input);

🧪 Tum search karo:

?search=<script>alert(1)</script>

⚠️ Agar sanitize nahi hua → alert pop → XSS ho gaya

---

2️⃣ innerHTML

➡️ Ye bhi bohot websites use karti hain — kisi element ke andar content daalne ke liye

document.getElementById("result").innerHTML = userInput;

🧪 Tum input bhejo:

?search=<img src=x onerror=alert(1)>

⚠️ Agar input sanitize nahi hua → image load fail → alert chala → XSS

---

3️⃣ insertAdjacentHTML()

➡️ Ye ek method hai jo HTML content ko kisi element ke andar inject karta hai

element.insertAdjacentHTML('beforeend', userInput);

🧪 Same payload:

?search=<svg onload=alert(1)>

⚠️ HTML ke through JavaScript chala → XSS

---

4️⃣ eval()

➡️ Ye sabse zyada dangerous function hai — isme tum kuch bhi string do, wo usko JavaScript samajh kar chala deta hai

eval(userInput);

🧪 Tum input bhejo:

?search=alert(1)

➡️ eval("alert(1)") chalega → XSS ho gaya

🛑 Note: Aaj kal ache developers eval() nahi use karte — lekin kabhi kabhi mil jata hai.

---

5️⃣ setTimeout("code") ya setInterval("code")

➡️ Isme agar string ke form mein input gaya to wo code ke tarah chal jata hai

setTimeout(userInput, 1000);

🧪 Input:

?search=alert(1)

➡️ 1 second baad JavaScript chalayega → XSS

---

### 🛡️ Kaise pata chalega ke koi sink use ho raha hai?

🔍 Tum view-source ya DevTools mein JS dekho:

Agar tumhein document.write, innerHTML, eval, etc dikhe…

Aur unke andar koi variable gaya ho (q, input, query, etc)

Aur wo variable location.search ya user input se aaya ho…

➡️ Toh samajh jao — sink + source = test karo XSS payload se 💣

---

JavaScript mein agar user ka input direct kisi sink (dangerous jagah) mein chala jaye bina check kiye, to attacker kuch bhi chala sakta hai — yehi hai DOM XSS

