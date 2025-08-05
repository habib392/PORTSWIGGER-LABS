# 🛡️ Lab: Web Shell Upload via Extension Blacklist Bypass

**Solved by: Habib**
**Approach: Practical Method (Bypass misleading official solution)**

---

## 🧠 Lab Ka Maqsad:

Website pr file upload functionality hai jo `.php` jaise dangerous extensions ko block karti hai. Lekin humne isko bypass krna hai aur ek **web shell upload** krni hai jo PHP execute kare aur Carlos ka secret file read krke return kare.

---

## ✅ Step-by-Step Process (Roman Urdu):

---

### 🔹 Step 1: PHP Web Shell File Banana

Sab se pehle apne system pr aik file banai `exploit.php` naam se. Is file mein yeh PHP payload likha:

```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

Yeh aik simple PHP shell hai jo `/home/carlos/secret` file read kar k output karega.

---

### 🔹 Step 2: Login Karna

Lab website pr gya aur login page open kiya. Credentials use kiye:

```
Username: wiener
Password: peter
```

Login hone ke baad **"My Account"** page open hua.

---

### 🔹 Step 3: Normal Image (.png) Upload Karna

Ek normal `.png` image upload ki (jaise avatar). BurpSuite background mein on tha, uss request ko capture kar liya.

Image successful upload hogayi thi, URL open kar k image dekh bhi li.

---

### 🔹 Step 4: Web Shell (.php) Upload Karne Ki Koshish

Ab `exploit.php` file upload karne ki koshish ki – lekin server ne block kar diya (error aaya ke yeh extension allowed nahi).

---

### 🔹 Step 5: BurpSuite Mein POST Request Analyze Karna

BurpSuite → HTTP History → `POST /my-account/avatar` wali request mili. Isko **2 dafa Repeater** mein bheja:

* **1st Repeater tab:** `.htaccess` upload ki
* **2nd Repeater tab:** exploit shell `.l33t` upload ki

---

### 🔹 Step 6: .htaccess Upload Karna

Pehli POST request mein yeh changes kiye:

* `filename="exploit.php"` ko change kiya `filename=".htaccess"`
* `Content-Type` ko change kiya `text/plain`
* Body content remove kar k yeh likha:

```
AddType application/x-httpd-php .l33t
```

**Note:** `.l33t` ki jagah kuch bhi custom likh sakte ho jaise `.khan`, `.test`, etc.

Phir request send ki aur dekha ke server ne `.htaccess` accept kar li (message: `The file avatars/.htaccess has been uploaded.`)

---

### 🔹 Step 7: exploit.l33t File Upload Karna

Dusri POST request (duplicate of original) mein sirf yeh 2 changes kiye:

* `filename="exploit.php"` → `filename="exploit.l33t"`
* `Content-Type` → `application/x-httpd-php`

Aur PHP shell ka content wohi rakha:

```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

Phir request send ki aur confirm kiya ke upload successful tha.

---

### 🔹 Step 8: Shell Ko Trigger Karna

BurpSuite main right click kiya repeater tab main phir show request in browser pr click kiya or request copy ki or browser main 
paste kr di or Carlos ka secret display hogaya

---

### 🔹 Step 9: Secret Submit Karna

Secret value copy ki aur lab page ke banner mein "Submit solution" box mein paste ki.

✅ Lab successfully **SOLVED**!

---

## 🎯 Lesson Learned:

* Kabhi bhi blindly PortSwigger ya kisi aur ka solution follow mat karo.
* Socho, samjho, test karo.
* GET request optional hoti hai, agar backend automatically shell run karta hai.
* `.htaccess` upload kaafi powerful weapon hai file upload bypasses mein.

---

## 🛠 Tools Used:

* BurpSuite Community Edition
* PortSwigger Academy Lab
* PHP Payload
* Browser

