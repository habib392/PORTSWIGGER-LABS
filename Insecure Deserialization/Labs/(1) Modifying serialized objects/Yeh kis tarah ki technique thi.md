## ⚡ Yeh Kis Tarah Ki Technique Thi?

Yeh **Serialization Attack** technique thi jisme humne ek **serialized PHP object** ko manipulate karke apni privileges escalate ki. Basically humne client-side cookie ko modify kiya aur server ne bina verify kiye usse accept kar liya. Is wajah se hum normal user se admin ban gaye.

---

## 🔥 Iss Lab Ki Khaas Baat Kya Thi?

* Cookie actually ek **serialized object** thi jo Base64 aur URL encoded tha.
* Admin flag ek simple boolean (`b:0`) ke form main store tha.
* Sirf ek chhoti si modification `b:0` → `b:1` karne se admin access mil gaya.
* Matlab developers ne session integrity ya signing implement nahi ki thi.

---

## 📌 Main Points of This Lab

1. Client-side serialized object session cookie.
2. Cookie decode kar ke modify karna possible tha.
3. Admin attribute as boolean stored tha.
4. Modification ke baad direct admin panel access mil gaya.
5. Carlos delete karke lab solve ho gaya.

---

## ❓ Kya Yeh Vulnerability Aaj Bhi Milti Hai?

Haan ✅ real world main yeh abhi bhi milti hai. Lekin modern apps zyada secure serialization methods use karte hain ya fir JWT tokens. Phir bhi agar koi purani PHP, Java, ya .NET app ho jahan insecure serialization ho rahi ho, toh aaj bhi aise cases milte hain.

---

## 👨‍💻 Yeh Vulnerability Developer Ki Wajah Se Hoti Hai?

Bilkul ✔️ yeh pura developer ki galti hoti hai:

* Agar developer client-side par sensitive data (admin rights, roles, etc.) store kar raha ho bina kisi **integrity check (HMAC / digital signature)** ke, toh yeh easily exploit ho sakta hai.
* Secure design yeh hai ke **sessions server-side maintain hon**, na ke cookies main direct objects store kiye jayein.

---

## ✨ Final Note:

Is lab ne sikhaaya ke agar developers careless ho aur client-side cookies main serialized objects daal dein bina protection ke, toh attacker easily admin ban sakta hai. Pentester ke liye yeh ek classic example hai **privilege escalation via insecure deserialization** ka.
