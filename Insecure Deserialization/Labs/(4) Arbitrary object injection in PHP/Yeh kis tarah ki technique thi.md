## 🔥 Lab Notes: Arbitrary Object Injection in PHP

### 1. Yeh kis tarah ki technique thi?

* Yeh **PHP Object Injection** technique thi.
* Isme attacker apna malicious serialized object bhejta hai jo server deserialize kar leta hai.
* Jab wo object destroy hota hai (`__destruct` function chal ke), tab attacker ke control wali action perform hoti hai.
* Is lab me humne file delete karwai (`unlink()` function) using this technique.

---

### 2. Is lab mein kya khaas baat thi?

* Khaas baat yeh thi ke server ne **serialized session cookies** use ki.
* Humne Burp se cookie decode ki aur ek PHP object dekha.
* Fir hume `/libs/CustomTemplate.php~` backup file mil gayi jisme source code expose hua.
* Us code me ek dangerous method `__destruct()` mila jo file delete karta tha.
* Isi ko exploit karke humne `/home/carlos/morale.txt` delete karwai.

---

### 3. Is lab ke main points kya thy?

* **Session cookies** → serialized objects the.
* **Source code leak** → `~` backup file ne code expose kar diya.
* **CustomTemplate class** → dangerous method `__destruct()` contain karti thi.
* **unlink() function** → file deletion karwa raha tha.
* **Exploit payload** → humne malicious object bana kar cookie me inject kiya.
* **Result** → Carlos ka `morale.txt` delete ho gaya, aur lab solve.

---

### 4. Developer ko kaunsi ghalti nahi karni chahiye thi?

* **Never unserialize untrusted data** → kabhi bhi user-controlled input ko directly unserialize na karo.
* **Backup files expose na karo** → server pe `.php~`, `.bak`, `.old` jaisi files public accessible nahi honi chahiye.
* **Sensitive methods safe rakho** → `__destruct()`, `__wakeup()`, `__toString()` me dangerous code nahi likhna chahiye jo attacker exploit kar sake.
* **Session management** → secure frameworks use karo jo cookies ko safe rakhe instead of custom serialization.

---

### 5. Kya yeh vulnerability aaj bhi milti hai?

👉 Haan, yeh abhi bhi exist karti hai lekin thodi rare hai.

* Purani PHP applications me bohot common thi.
* Aaj kal bhi agar developers careless ho aur direct `unserialize($_COOKIE)` ya `unserialize($_POST)` use karein, to exploit possible hai.
* Agar code open source ho aur developers magic methods (`__destruct`, `__wakeup`) galat use karein, to attack hota hai.

---

### 6. Kya yeh developer ki wajah se hoti hai?

✅ 100% developer ki ghalti hoti hai.

* Agar developer secure coding practices follow kare to aisi vulnerability nahi aati.
* Ye problem tab hoti hai jab developer **user input ko blindly trust** kar leta hai.
* Isliye yeh ek **insecure coding mistake** hai.

---

## ✅ Conclusion

Is lab ne hume yeh sikhaya ke:

* Source code leaks bohot dangerous hote hain.
* Serialized objects ko user-controlled input ke sath kabhi trust nahi karna chahiye.
* PHP ke magic methods exploit ke liye easy entry point hote hain.
* Secure frameworks aur validation use karna must hai.
