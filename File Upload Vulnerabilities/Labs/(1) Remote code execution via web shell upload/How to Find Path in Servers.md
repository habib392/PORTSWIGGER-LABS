### **Real Websites Mein File Destination Kaise Pata Lagta Hai?**

Server kahan files save kar raha hai, ye directly nahi batata, lekin kuch clues aur testing se hum isay figure out kar sakte hain. Ye raha process:

#### **1. HTTP Responses Ko Analyze Karo**
- Jab tu koi file upload karta hai (jaise image ya document), website ka response check karo. Kabhi-kabhi response mein file ka path ya location ka hint milta hai.
- **Misal**: Agar tu ek image upload karta hai aur response mein kuch aisa milta hai:
  ```json
  {"message": "File uploaded successfully", "url": "/uploads/images/yourfile.jpg"}
  ```
  To isse pata chalta hai ke file `/uploads/images/` folder mein save hui.
- **Tool**: Burp Suite ya browser ke Developer Tools (F12 > Network tab) use karo taake response dekho.

#### **2. Uploaded File Ko Access Karne Ki Koshish Karo**
- Jab tu file upload karta hai, website aksar us file ko publicly accessible URL pe serve karti hai (jaise profile picture ke liye).
- Browser mein ya Burp Suite ke Repeater mein us URL ko check karo. URL ka structure dekh ke path guess kar sakte ho.
- **Misal**: Agar uploaded image ka URL hai `https://website.com/files/avatars/yourimage.jpg`, to shayad server files ko `/files/avatars/` mein save kar raha hai.
- Is lab ki tarah, jab tune image upload ki, wo `/files/avatars/<filename>` pe mili, jo hint tha ke server wahan files store karta hai.

#### **3. Error Messages Ka Faida Uthao**
- Websites kabhi-kabhi error messages mein sensitive info leak karti hain, jaise file path.
- **Misal**: Agar tu ek invalid file upload karta hai aur error aata hai:
  ```
  Error: Could not save file to /var/www/uploads/
  ```
  To ye direct batata hai ke server `/var/www/uploads/` mein files save karta hai.
- Real-world mein developers aise errors hide karte hain, lekin agar misconfiguration ho, to ye clue mil sakta hai.

#### **4. File Name aur Extension Ke Sath Experiment Karo**
- Alag-alag file names ya extensions ke sath upload kar ke dekho ke server kaise behave karta hai.
- **Misal**: Agar tu `test.jpg` upload karta hai aur wo `/uploads/test.jpg` pe milta hai, to shayad `test.php` bhi `/uploads/test.php` pe jayega.
- Kabhi-kabhi server files ko rename karta hai (jaise `test.jpg` ko `123456789.jpg`), lekin path same rehta hai. Isay Burp Suite mein requests dekh ke confirm karo.

#### **5. Directory Enumeration aur Guessing**
- Agar direct path nahi mil raha, to common folder names guess karo jo websites files store karne ke liye use karti hain, jaise:
  - `/uploads/`
  - `/files/`
  - `/images/`
  - `/avatars/`
  - `/media/`
- Tools jaise **dirb**, **gobuster**, ya **ffuf** use kar ke server pe common directories scan karo. Ye tools possible folder paths dhoond sakte hain.
- **Misal**: `gobuster dir -u https://website.com -w wordlist.txt` chalao, jahan wordlist mein common folder names hote hain.

#### **6. Source Code ya Leaked Info Check Karo**
- Agar website ka source code (HTML, JavaScript) dekho, to kabhi-kabhi file paths ya upload directories ke references milte hain.
- **Misal**: JavaScript file mein AJAX request ho sakta hai jo `/uploads/` ko point karta ho.
- Ya phir, agar website ka koi part open-source hai ya GitHub pe leak hua hai, to configuration files (jaise `.env` ya `config.php`) mein upload directory ka pata chal sakta hai.

#### **7. File Upload Request Ko Manipulate Karo**
- Burp Suite ke Proxy ya Repeater se file upload ke HTTP request ko intercept karo aur dekho ke server kaise handle karta hai.
- Kabhi-kabhi request mein parameters hote hain jo destination path batate hain, jaise:
  ```http
  POST /upload HTTP/1.1
  Content-Type: multipart/form-data
  ...
  Content-Disposition: form-data; name="destination"
  /uploads/images/
  ```
- Isay manipulate kar ke confirm karo ke server files kahan save karta hai.

#### **8. Server Behavior Se Guess Karo**
- Agar server files ko execute karta hai (jaise `.php`), to shayad wo web root directory (jaise `/var/www/html/`) ya uske subfolders mein save kar raha hai.
- Agar files publicly accessible hain, to wo shayad `/public/`, `/static/`, ya `/uploads/` jaise folders mein honge.
- Real-world mein, servers cloud storage (jaise AWS S3) bhi use karte hain, to URL se bucket ya path ka idea mil sakta hai.

---

### **Lab Mein Path Kaise Pata Chala?**
Is lab mein `/files/avatars/` path ka pata isliye chala kyun ke:
- Jab tune ek random image upload ki, uska preview account page pe dikhaya gaya.
- Burp Suite ke Proxy > HTTP History mein tune dekha ke image ka GET request tha: `GET /files/avatars/<your-image>.jpg`.
- Isse clue mila ke server uploaded files ko `/files/avatars/` mein store karta hai.
- Phir tune `exploit.php` upload ki aur same path (`/files/avatars/exploit.php`) pe request bheji, jo kaam kar gayi.

Real-world mein bhi same logic kaam karta hai, bas thodi zyada testing aur tools ki zarurat hoti hai kyun ke websites itni openly paths nahi deti.

---

### **Real-World Mein Challenges**
- **Validation**: Real websites file types check karti hain, to `.php` upload karna asaan nahi hota. Tujhe bypass techniques (jaise double extensions `.php.jpg` ya MIME type manipulation) use karni padti hain.
- **Hidden Paths**: Servers files ko random ya obfuscated paths (jaise `/uploads/123456789/`) mein save kar sakte hain.
- **Access Restrictions**: Kabhi-kabhi uploaded files publicly accessible nahi hoti, to tujhe server-side execution ya directory traversal jaise attacks try karne padte hain.
- **Logging aur Monitoring**: Real websites suspicious activity (jaise `.php` upload) detect kar sakti hain, to stealthy rehna padta hai.

---

### **Tools Jo Help Karte Hain**
- **Burp Suite**: HTTP requests intercept aur analyze karne ke liye.
- **Dirb/Gobuster/Ffuf**: Common directories dhoondne ke liye.
- **Postman**: Manual HTTP requests test karne ke liye.
- **Browser Dev Tools**: Responses aur network activity dekhne ke liye.
- **Metasploit**: Agar web shell upload ho jaye, to usay exploit karne ke liye.


