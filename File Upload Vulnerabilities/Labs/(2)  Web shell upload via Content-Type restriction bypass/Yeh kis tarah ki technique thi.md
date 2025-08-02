### ❓1. **Is lab ke main points kya thay?**

* Server **image file upload** allow karta tha (`image/jpeg`, `image/png`)
* Server sirf **Content-Type header** check kar raha tha — file ka actual content nahi
* Tumne `Content-Type` ko manually change karke **PHP shell** upload kar di
* Us shell se `/home/carlos/secret` ka content nikal liya

---

### ❓2. **Is se kya seekha?**

* **Client-side validation pe kabhi bharosa nahi karna chahiye**
* Sirf headers ya extensions check karna kaafi nahi hota
* Burp Suite se headers modify karke servers ko trick kiya ja sakta hai
* **MIME type spoofing** se file restrictions bypass ki ja sakti hain

---

### ❓3. **Yeh kis tarah ki technique thi?**

> ✅ **MIME Type Bypass / Content-Type Restriction Bypass**

* Yeh ek **input validation bypass** hai
* Attacker ne `image/jpeg` dikhaya, lekin file `PHP shell` thi
* **"What you see is not what you get"** — server ko dhoka diya gaya

---

### ❓4. **Developer ki kya ghalti thi?**

* Developer ne socha ke **Content-Type header** kaafi hai file validate karne ke liye
* Usne **file content ya extension validate nahi kiya**
* Developer ne **security ko client input pe chhod diya** (bad practice)

---

### ❓5. **Server ki kya ghalti thi?**

* Server ne:

  * PHP file ko **save bhi kar liya**
  * Aur **web-accessible path** pe rakh diya (`/files/avatars/exploit.php`)
* Server ne koi:

  * **File scanning** nahi ki (e.g., using `finfo`)
  * **Extension check** nahi ki
  * **Execution block** nahi kiya `.php` files ka

---

### ❓6. **MIME Type Headers kya hotay hain?**

* MIME ka matlab hai: **Multipurpose Internet Mail Extensions**
* Yeh browser ko batata hai file kis type ki hai
* Example:

  * `image/jpeg` → JPEG image
  * `application/pdf` → PDF file
  * `application/x-php` → PHP script
* Attacker ne is header ko **fake** karke dhoka diya server ko

---

### ❓7. **Mujhe is lab se kya seekhna chahiye?**

#### 🔐 As a future pentester:

* Burp Suite se headers modify karna seekho
* **Upload-based attacks** ko deeply samjho:

  * Content-Type bypass
  * Extension bypass (`.php.jpg`)
  * Null byte injection
* Developer aur server side validations dono identify karna seekho
* Jab bhi file upload mile kisi site pe — **alert ho jao**, yeh ek common attack surface hai
