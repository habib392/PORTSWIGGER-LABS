### 📌 Yeh Technique Kya Thi?

Yeh technique **Insecure Deserialization + PHP Loose Comparison** thi. Matlab humne serialized object ke andar values aur unke data types modify karke system ko confuse kiya. PHP ka purana comparison system (PHP 7.x aur usse pehle) string aur integer ko barabar samajh leta hai agar value match ho jaaye. Is trick ka use karke authentication bypass kiya jaa sakta hai.

---

### ⭐ Lab ki Khaas Baat

* Sirf value badalne se kaam nahi hota tha → datatype bhi change karna zaroori tha.
* Yeh lab specifically PHP ke **type juggling** behavior (string vs integer comparison) exploit kar raha tha.
* Isme ek chhoti si datatype change se normal user ko **admin access** mil gaya.

---

### 🔑 Main Points

1. Session cookie ek serialized PHP object tha.
2. Username aur access token dono client-side object me store the.
3. Authentication check karne ke liye server ne datatype strict nahi rakha.
4. Loose comparison ka misuse karke admin account access mil gaya.
5. Yeh vulnerability bohot dangerous hai kyunki directly **authorization bypass** allow karti hai.

---

### ❌ Developer ki Ghalti

* Server ne **serialized objects client-side store kiye**, jo kabhi secure practice nahi hai.
* Loose comparison (`==`) use kiya instead of strict comparison (`===`).
* Input aur datatype validation nahi kiya.
* Secure session management implement nahi kiya.

---

### 🔍 Aaj Bhi Milti Hai?

Haan, insecure deserialization aur type juggling aaj bhi real-world me mil jaati hai. Specially PHP aur kuch purane frameworks use karne wali applications me.

---

### ⚡ Developer ki Wajah Se Hoti Hai?

Bilkul. Yeh bug 100% developer ki wajah se hoti hai. Agar developer:

* Strict comparison use karein (`===`),
* Proper input aur datatype validation karein,
* Aur sensitive data (jaise session objects) client-side store na karein,
  …toh yeh vulnerability kabhi na hoti.

---

✅ **Final Note:**
Yeh lab sikhaata hai ke kabhi kabhi coding languages ke chhote quirks (jaise PHP ka type juggling) bohot bade security holes ban jaate hain. Penetration tester ko hamesha datatype aur encoding par focus karna chahiye. 🔥
