## 💥 Lab Name: Web Shell Upload via Obfuscated File Extension

---

### 🔍 Yeh Kis Tarah Ki Technique Thi?

Yeh ek **file upload bypass** technique thi — jahan developer ne `.php` jaisi dangerous file extensions block ki hui thi, lekin humne uss block ko **null byte injection (%00)** ke zariye bypass kiya.

* `%00` ka matlab hota hai **null byte**, jo system ko yeh batata hai ke string yahin khatam ho gayi.
* Humne `exploit.php%00.jpg` upload kiya. Server ne `.php` tak hi padha aur `.jpg` ignore kar diya.
* Is tarah `.php` file upload ho gayi aur run bhi ho gayi — **Remote Code Execution** mil gaya!

---

### 🧠 Is Lab ke Main Points Kya Thay?

1. **File Extension Blocked Thi** – Website ne `.php` block kiya hua tha.
2. **MIME Type Check Bhi Ho Sakta Tha** – Lekin bypass ho gaya.
3. **Humne Null Byte Injection Use Kiya** – `%00` se extension validation confuse ho gaya.
4. **Shell Upload Hui Aur Run Bhi Hui** – Humne Carlos ka secret file read kar liya.
5. **BurpSuite se Request Modify Ki** – Yeh sab normal browser se possible nahi tha.

---

### 🌟 Iss Lab Mein Kya Khaas Baat Thi?

* **Validation front-end ya client-side par thi**, lekin backend par solid check nahi tha.
* Humne `filename="exploit.php%00.jpg"` likh kar backend ko trick kar diya.
* Yeh technique old hai lekin powerful hai — agar developer ka dhyaan na ho toh aaj bhi kaam karti hai.
* **Sirf extension block karna kaafi nahi hota**, MIME type aur file content validation bhi zaroori hai.

---

### ❓ Kya Yeh Vulnerability Aaj Bhi Milti Hai?

✅ **Haan, milti hai** — specially:

* **Old CMS systems (e.g. Joomla, old WordPress themes/plugins)**
* **Local businesses ki websites**
* **Custom-built admin panels jahan proper sanitization nahi hoti**

Yeh rare hai lekin agar mile toh **full server access** ka chance hota hai.

---

### 🚨 Kya Yeh Vulnerability Developer Ki Galti Hoti Hai?

💯 **Bilkul!** Yeh 100% developer ki galti hoti hai. Common mistakes:

1. Sirf **file extension check** karna (like `.php`, `.exe`, etc.)
2. **MIME type verify nahi karna**
3. **Filename sanitization** na karna
4. Uploaded file ko directly web accessible folder mein rakh dena (like `/uploads/`, `/images/avatars/` etc.)

Agar developer yeh sab properly handle karein, toh aise bypass mushkil ho jaate hain.

---

### 📌 Summary Table:

| Point              | Explanation                                |
| ------------------ | ------------------------------------------ |
| Technique          | Null Byte Injection (%00)                  |
| Vulnerability Type | File Upload Bypass                         |
| Exploitation Tool  | BurpSuite                                  |
| Real Impact        | Web Shell Upload → Remote Code Execution   |
| Cause              | Poor backend validation by developer       |
| Still Exists?      | Yes, in outdated or poorly developed sites |
