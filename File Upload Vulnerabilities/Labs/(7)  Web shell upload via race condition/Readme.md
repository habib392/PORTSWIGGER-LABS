## 🔥 **Lab ka Naam:** Web Shell Upload via Race Condition

**Goal:** Carlos ka secret file `/home/carlos/secret` ka content nikalna.

---

### ✅ Step 1: Web Shell File Banao

Apne system pe aik file banao jiska naam ho:
`exploit.php`

Iske andar yeh code daalo:

```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

**Matlab:** Jab yeh file browser mein open hogi, toh Carlos ka secret print ho jayega screen pe.

---

### ✅ Step 2: Login karo

Lab open karo → Login credentials use karo:

* **Username:** wiener
* **Password:** peter

Login hone ke baad tum "My Account" page pe chalay jao.

---

### ✅ Step 3: File Upload Try karo

"My Account" page pe ek **upload image** ka option hoga.
Wahan se apni `exploit.php` file choose karo aur upload karo.

Server error dega:

```
Sorry, only JPG & PNG files are allowed
```

**Matlab:** Server tumhari PHP file reject kar raha hai kyunki woh sirf image allow kar raha hai.

---

### ✅ Step 4: Burp Suite ka Use karo

* Burp Suite open karo
* **Proxy > HTTP history** mein jao
* Jo file upload ki request hui thi uska path hoga:
  `POST /my-account/avatar`

Us request ko **Repeater** mein send kar do.

---

### ✅ Step 5: Valid JPG Upload karo

"My Account" page pe jao dobara
Iss baar koi normal JPG image upload karo (jaise cat.jpg)

Upload ho jayegi.

Ab jao Burp Suite ke **HTTP history** mein
Dekho ek request hogi:
`GET /files/avatars/cat.jpg`
Yeh prove karta hai ke image `avatars/` folder mein save ho rahi hai.

---

### ✅ Step 6: GET Request Prepare karo

Repeater mein ek nayi request banao:

```
GET /files/avatars/exploit.php HTTP/1.1
Host: <tumhara-lab-url>
Connection: close
```

Iska matlab: Jab `exploit.php` upload ho jaye, toh yeh request usay browser mein open karegi taake Carlos ka secret mil sake.

---

### ✅ Step 7: Race Condition Plan Banao

Yeh trick yahan kaam karti hai:

* Jab tum `POST /my-account/avatar` se PHP file upload karte ho
* Toh woh server pe turant save ho jati hai
* Lekin validation (jaise virus scan) baad mein hota hai
* Us scan/delay ke beech mein hum us file ko access kar lenge

---

### ✅ Step 8: Manual Race Condition Trigger karo (No Turbo Intruder)

1. `POST /avatar` request ko Repeater mein rakho (jisme `exploit.php` upload ho)
2. `GET exploit.php` request ko 5 ya 6 baar Repeater mein copy karo
3. Sab GET requests ko **select karo**
4. `+` pe click karo (tab group banane ka option aayega)
5. **"Create tab group"** pe click karo
6. Color assign karo (optional), OK karo
7. Ab Repeater ke neeche new option aayega:
   ➤ `Send group in parallel`

**Isko click karo!**

---

### ✅ Step 9: Dekho Response

Agar tumhara timing sahi raha, toh kisi ek request ka response 200 hoga
Aur andar likha hoga Carlos ka secret, jaise:

```
tq3J73M9lkIjsdfhA0sd9
```

Yahi **secret** lab ko solve karega.

---

### ✅ Step 10: Secret Submit karo

Lab ke top banner pe ek input box hoga
Wahan secret paste karo → **Submit** karo → 🎉 **LAB SOLVED**

---

## 💡 Real-Life Penetration Testing Learning:

* Jab file validation **asynchronously hoti hai** (matlab baad mein hoti hai), wahan race condition ka risk hota hai
* Agar file short time ke liye server pe accessible ho jaati hai, toh attacker usko ussi moment pe access karke exploit kar sakta hai
* **Turbo Intruder** ya **Repeater Parallel Send** se yeh race jeeti ja sakti hai

