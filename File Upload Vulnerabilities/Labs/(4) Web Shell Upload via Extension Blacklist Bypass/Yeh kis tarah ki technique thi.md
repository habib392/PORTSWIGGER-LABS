## 🔍 Yeh kis tarah ki technique thi?

Yeh technique hoti hai:

### 🔥 "Extension Blacklist Bypass"

Iska matlab hota hai ke:

> Server kuch extensions block karta hai (jaise `.php`, `.jsp`, `.asp`) — kyunke yeh dangerous hote hain — lekin developer sirf extension ka naam check kar raha hota hai, **us file ka behavior nahi**.

Hum is limitation ka **bypass** karte hain:

* Ya to kisi **allowed extension ko PHP jaisa treat** karwa ke (jaise `.l33t`, `.phtml`)
* Ya phir **server ke config ko manipulate** karke `.htaccess` se `.l33t` ko PHP bana dete hain.

---

## 🎯 Is Lab ke Main Points:

### ✅ 1. **Server Extension Blacklist Use Kar Raha Tha**

Server `.php` ko block kar raha tha — lekin yeh bas extension check kar raha tha, file content ya MIME-type nahi.

### ✅ 2. **Apache Web Server Use Ho Raha Tha**

Response headers se pata chala ke server `Apache/2.4.41 (Ubuntu)` hai — iska matlab `.htaccess` kaam karega.

### ✅ 3. **.htaccess File Allowed Thi**

Server ne `.htaccess` upload karne diya, jismein humne bola:

```apache
AddType application/x-httpd-php .l33t
```

Matlab: `.l33t` extension wali file ko **PHP ki tarah execute karo**.

### ✅ 4. **Content-Type Header Manually Control Kiya**

Humne file ka `Content-Type` header change karke server ko trick kiya ke yeh sirf plain text hai — is tarah `.htaccess` file server ne accept kar li.

### ✅ 5. **Web Shell Upload Aur Execution**

Ek `.l33t` extension wali file upload ki jis mein PHP shell tha — server ne usay `.php` ki tarah execute kiya aur secret file read kar li.

---

## 🌟 Is Lab Ki Khaas Baat Kya Thi?

### 🔥 Realistic Attack Chain:

Yeh koi unrealistic lab nahi tha — bilkul real-world mein hone wali attack chain thi:

* **Web shell upload** → server access
* **.htaccess abuse** → behavior change
* **Sensitive file read** → credential theft / RCE

### 🔥 Misleading Protection:

Server ne protection laga rakhi thi — lekin sirf **naam ki**. Jo developers karte hain:

```php
if ($extension == 'php') {
    block file
}
```

Bas! Yeh koi solid protection nahi hoti.

### 🔥 Do Files Upload Karni Parti Hain:

Is lab mein special baat yeh thi ke humein **do alag files upload karni padi**:

1. `.htaccess` → config change karne ke liye
2. `exploit.l33t` → shell execute karne ke liye

---

## 🏴‍☠️ Kya yeh vulnerability aaj bhi milti hai?

**Haan! 100% milti hai — agar developer careless ho.**

### ✅ Real-World Scenarios:

* Old CMS systems (WordPress plugins, Joomla, etc.)
* Misconfigured Apache servers
* Custom file upload features without proper validation

### ✅ Real Case Example:

* 2021 mein ek **real estate website** pe attacker ne `.phtml` file upload kar li — aur pura control le liya server ka. Unhone sirf `.php` block kiya tha, lekin `.phtml` execute ho gaya.

---

## 👨‍💻 Yeh Vulnerability Developer ki ghalti ki wajah se hoti hai?

### ✅ 100% Developer Ki Galti Hoti Hai!

**Kyun?**

* Sirf extension check kar lena secure nahi hota.
* Proper validation nahi hoti: file content, MIME-type, execution rights
* `.htaccess` ko upload hone dena bohat badi security risk hai
* Web server (jaise Apache) ko galat tarike se configure karna

---

## 🛡️ Protection Tips (Developers ke liye):

1. ✅ Sirf extension check nahi, **MIME-type + file content** bhi verify karo
2. ✅ Dangerous file types ko bilkul allow hi mat karo
3. ✅ `.htaccess`, `.phtml`, `.php`, `.php5`, `.phar`, etc. **sab block karo**
4. ✅ File uploads ko alag directory mein rakho jo execute nahi hoti (`no-exec`)
5. ✅ Web server config secure karo — jaise Apache mein `.htaccess` disable karo

---

## 💡 Final Thought:

**Yeh lab humein sikhata hai:**

> **Security is not about checkboxes — it's about understanding behavior.**
> Sirf extension block kar dena enough nahi hota, jab tak tum system ki working ko **first principles** se nahi samajhte.
