## 🔐 Lab: Authentication Bypass via Encryption Oracle

### 🧑‍💻 Step 1: Login with "Stay logged in" Option

* Login page pe jaa kar credentials use karo:
  **Username:** `wiener`
  **Password:** `peter`
* "Stay logged in" ka option enable karna mat bhoolna.

---

### 💬 Step 2: Post a Comment with Invalid Email

* Kisi bhi blog post pe comment karo.
* Name aur Comment kuch bhi likho, lekin **email** galat daalo.
  Jaise: `rock.sacom` (yani `@` na ho).
* Comment post kar do.

---

### 🔎 Step 3: Observe Cookies in Burp

* BurpSuite mein POST request dekho (comment wali).
* Usmein `stay-logged-in` cookie hogi.
* Phir GET request dekho (redirect ke baad wali) — usmein `notification` cookie bhi add hogi.

---

### 📂 Step 4: Send Requests to Repeater and Rename Tabs

* POST `/post/comment` → Repeater mein bhej kar tab ka naam **Encrypt** rakho.
* GET `/post?postId=1` → Tab ka naam **Decrypt** rakho.

---

### 🔓 Step 5: Decrypt to Reveal Input

* GET (Decrypt) request send karo.
* Response mein neeche likha aayega:
  `Invalid email address: rock.sacom`
* Yeh prove karta hai ke `notification` cookie decrypt ho rahi hai.

---

### 📋 Step 6: Reveal Format of stay-logged-in Cookie

* Notice karo ke POST aur GET dono requests mein `stay-logged-in` cookie same hai.
* Is cookie ko copy karo.
* GET (Decrypt) request mein `notification` cookie ko replace kar do is `stay-logged-in` cookie se.
* Request send karo.
  Output mein milega:
  `wiener:1753861760109`

---

### ✏️ Step 7: Modify and Re-Encrypt with Admin Username

* `wiener` ko replace karo `xxxxxxxxxadministrator` se (9 x lagao taake block size match ho).
  Result: `xxxxxxxxxadministrator:1753861760109`
* Is value ko POST (Encrypt) request ke `email` field mein daal do.
* Request send karo.
  Response mein nayi `notification` cookie milegi.

---

### 🧠 Step 8: Decode, Trim Prefix, and Re-Encode

* Is new notification cookie ko Burp Decoder mein:

  1. **URL Decode** karo
  2. **Base64 Decode** karo
* Ab manually first **32 characters** delete karo (yeh `"Invalid email address: "` aur padding hai).
* Phir:

  1. Base64 encode karo
  2. URL encode karo
* Nayi encoded cookie ready hai.

---

### 🧪 Step 9: Test Decrypted Value

* GET (Decrypt) request mein notification cookie ko replace karo apni nayi encoded value se.
* Request send karo.
  ✅ Agar response mein aayega: `administrator:1753861760109`
  ➤ To kaam ho gaya.

❌ Agar 500 error aaya:
➤ To base64 ya URL encoding dubara sahi karo, ya exact 32 characters delete karo.

---

### 🚪 Step 10: Use Forged Cookie to Login as Admin

* HTTP history se GET `/` request ko Repeater mein bhejo.
* Usmein:

  * `session` cookie ka pura parameter delete kar do.
  * `stay-logged-in` cookie ko apni forged admin cookie se replace karo.
* Request send karo.
* Right-click → **"Show response in browser"**
  ➤ Copy the URL, open it in browser → **Admin panel show ho jayega**

---

### 🧹 Step 11: Delete Carlos

* Browser ya Repeater mein:
  `/admin/delete?username=carlos`
  ➤ Request send karo.

✅ **Lab Solved! 🎉**
