### **Lab Kya Hai?**
Tujhe ek website di jati hai jo vulnerable hai, matlab usmein security flaws hain. Tera kaam hai us flaw ka faida utha kar ek task complete karna, jo is lab mein hai: `/home/carlos/secret` file ka content nikalna aur submit karna.

**Lab ka Scene**:
- Website mein ek image upload function hai (jaise profile picture upload karna).
- Ye function files ko check nahi karta, yani koi bhi file upload ho sakti hai (chahe wo image ho ya kuch aur).
- Tujhe is weakness ka faida utha kar ek malicious PHP file upload karni hai, jo server pe commands run karegi aur secret file ka content dikhayegi.

---

### **Server Ka Kya Scene Hai?**
Server ek computer hai jo website ko host karta hai. Ye files store karta hai, requests handle karta hai, aur users ko data bhejta hai. Is lab mein server ka issue ye hai ke wo koi bhi file accept kar raha hai bina validation ke.

- **Server Konsi File Accept Kar Raha Hai?**
  - Is lab mein server **koi bhi file** accept kar raha hai, chahe wo `.jpg`, `.png`, ya `.php` ho. Koi validation nahi hai, yani server file ke type, naam, ya content ko check nahi karta.
  - Normally, ek secure server sirf allowed file types (jaise `.jpg`, `.png`) accept karta hai aur dangerous files (jaise `.php`) block karta hai.

- **Konsi File Accept Nahi Karni Chahiye?**
  - Server ko **executable files** jaise `.php`, `.asp`, `.jsp` ya `.py` accept nahi karni chahiye, kyun ke ye files code run kar sakti hain. Agar attacker inhe upload kar de aur server inhe execute kare, to wo server ka control le sakta hai.
  - Is lab mein server `.php` files accept kar raha hai, jo ek badi security mistake hai.

---

### **Server Ki Ghalti Hai Ya Developer Ki?**
- **Developer Ki Ghalti**: Developer ne file upload function banate waqt proper validation nahi lagayi. Matlab, unhone server ko nahi bataya ke sirf specific file types (jaise images) allow karni hain. Is wajah se server koi bhi file accept kar raha hai.
- **Server Ki Ghalti**: Server bhi galat configure hai, kyun ke wo `.php` files ko execute kar raha hai. Ek secure server dangerous file types ko execute nahi karta, balke error deta hai ya unhe plain text ke taur pe serve karta hai.

Dono ka combination is vulnerability ka sabab hai. Developer ne validation nahi lagayi, aur server `.php` files ko run karne ke liye configured hai.

---

### **Exploit.php File Kyun Banayi?**
- **Exploit.php Kya Hai?**: Ye ek malicious PHP file hai jo tujhe server pe commands run karne degi. Is lab mein tujhe `/home/carlos/secret` file ka content chahiye, to is file mein ek simple PHP code likha jata hai jo ye kaam karta hai.
- **PHP File Kya Hoti Hai?**: PHP ek server-side programming language hai. Jab server ko `.php` file milti hai, to wo uske andar ka code execute karta hai aur result client (tumhare browser) ko bhejta hai. Ye files dynamic content generate karne ke liye use hoti hain, lekin agar attacker inhe upload kar de, to wo malicious code run kar sakta hai.
- **Kyun Banayi?**: Tujhe ek aisi file chahiye jo server pe upload ho aur `/home/carlos/secret` file ka content read kare. Isliye hum `exploit.php` banate hain.

---

### **Server PHP File Ko Kaise Accept Kar Raha Hai Jab Wo JPG/PNG Block Kar Raha Hai?**
- Is lab mein server **kuch bhi block nahi kar raha**. Lab ka description kehta hai ke image upload function mein **koi validation nahi hai**. Yani, chahe tu `.jpg`, `.png`, ya `.php` upload kare, server sab accept kar lega.
- Real-world mein servers image files (.jpg, .png) allow karte hain aur `.php` jaise files block karte hain, lekin is lab mein ye flaw hai ke server koi check hi nahi karta.

---

### **Code Ka Matlab: `<?php echo file_get_contents('/home/carlos/secret'); ?>`**
Chalo, is code ko ek ek part samjhte hain:

1. **<?php ... ?>**:
   - Ye PHP code ka start aur end tag hai. Iske andar jo bhi likha jata hai, wo PHP code hota hai jo server execute karta hai.

2. **file_get_contents('/home/carlos/secret')**:
   - Ye ek PHP function hai jo kisi file ka content read karta hai.
   - `/home/carlos/secret` ek file path hai jo server ke filesystem mein mojood hai. Ye wo file hai jiska content tujhe chahiye.
   - Ye function us file ka poora content (text) ek string ke roop mein return karta hai.

3. **echo**:
   - Ye PHP command hai jo kisi bhi data ko output karta hai, yani browser ko bhejta hai.
   - Is case mein, `file_get_contents` jo content return karta hai (secret file ka text), wo `echo` browser ke response mein dikhata hai.

**Full Matlab**: Jab tu `exploit.php` upload karta hai aur usay request karta hai, server is file ko execute karta hai. Ye code server se kehta hai ke `/home/carlos/secret` file ka content padho aur mujhe response mein dikhao. Result mein tujhe wo secret code milta hai jo lab solve karne ke liye submit karna hai.

---

### **Path Ka Matlab: `GET /files/avatars/exploit.php HTTP/1.1`**
Ye ek HTTP request hai jo browser server ko bhejta hai. Chalo iske har part ko samjhte hain:

1. **GET**:
   - Ye HTTP request ka method hai. GET ka matlab hai ke tum server se koi resource (jaise file ya page) mang rahe ho.
   - Dusre methods hote hain jaise POST (data submit karne ke liye), lekin is case mein hum sirf file ka content mang rahe hain, isliye GET use hota hai.

2. **/files/avatars/exploit.php**:
   - Ye server pe us file ka path hai jahan tumhari uploaded `exploit.php` file store hui hai.
   - Lab mein jab tu koi file upload karta hai (jaise image ya PHP file), wo server ke `/files/avatars/` folder mein save hoti hai.
   - Yani, jab tu `exploit.php` upload karta hai, wo is path pe available hoti hai: `/files/avatars/exploit.php`.

3. **HTTP/1.1**:
   - Ye HTTP protocol ka version hai jo request ke sath use ho raha hai. Ye batata hai ke request ka format kaisa hai. Tumhe iski zyada tension lene ki zarurat nahi, ye standard part hai.

**Full Matlab**: Jab tu `GET /files/avatars/exploit.php HTTP/1.1` request bhejta hai, tu server se keh raha hai ke mujhe `/files/avatars/exploit.php` file do. Server is file ko dekhta hai, aur kyun ke ye ek PHP file hai, wo iska code execute karta hai aur result (secret file ka content) tujhe response mein bhejta hai.

---

### **Summary of Lab**
- **Kya Karna Hai**: Ek PHP file (`exploit.php`) banani hai, usay upload karna hai, aur phir us file ko request kar ke secret code nikalna hai.
- **Kyun Kaam Kar Raha Hai**: Server koi validation nahi karta, isliye `.php` file upload aur execute ho jati hai.
- **Developer Ki Ghalti**: Unhone file type check nahi kiya.
- **Server Ki Ghalti**: Wo `.php` files ko execute kar raha hai.
- **Code Ka Kaam**: `exploit.php` mein likha code secret file ka content read karta hai aur response mein dikhata hai.
- **GET Request**: Ye server se file mangne ka tareeka hai, aur `/files/avatars/exploit.php` wo path hai jahan file store hai.

---

### **Step-by-Step Lab Solve Karne Ka Tariqa (Phir Se Short Mein)**
1. **Burp Suite Chalao**: Proxy on karo, browser configure karo.
2. **Login Karo**: Username `wiener`, password `peter` se login karo.
3. **Image Upload Karo**: Ek random `.jpg` upload karo aur confirm karo ke wo `/files/avatars/` mein save hui.
4. **PHP File Banayo**: `exploit.php` mein ye code daalo:
   ```php
   <?php echo file_get_contents('/home/carlos/secret'); ?>
   ```
5. **PHP File Upload Karo**: Avatar upload function se `exploit.php` upload karo.
6. **Request Bhejo**: Burp Repeater mein `GET /files/avatars/exploit.php` request bhejo. Response mein secret code milega.
7. **Secret Submit Karo**: Code lab ke banner mein submit karo.

---
