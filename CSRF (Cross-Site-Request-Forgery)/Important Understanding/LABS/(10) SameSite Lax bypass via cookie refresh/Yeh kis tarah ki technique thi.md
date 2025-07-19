## 🔥 **Lab Summary: SameSite Lax Bypass via Cookie Refresh**

### ✅ **Kya Seekha?**

Yeh lab ne tumhe yeh sikhaya ke:

> Agar website session cookies ko `SameSite=Lax` per chhodti hai, toh tum **CSRF attack** ko bypass kar sakte ho agar **cookie refresh** ho jaye via same-site `GET` request jaise `/social-login`.

---

## 🧠 **Yeh Kis Tarah Ki Technique Thi?**

### ✳️ **CSRF (Cross-Site Request Forgery) Attack:**

Humne victim ke browser se **bina uske jaane** ek `POST request` bheji uski authenticated session ke sath — **email change karne ke liye**.

### ✳️ **SameSite Lax Restriction Bypass:**

Normally, `SameSite=Lax` cookies **POST** requests main nahi jaati **cross-site** origin se.

Lekin:

* Agar cookie **GET request** se refresh ho jaye (e.g. `/social-login`),
* Aur uske turant baad **POST request bheji jaye**,
* Toh browser naye session ke sath **authenticated POST request** accept kar leta hai.

---

## ⚙️ **Lab Ko Kaise Samjha Aur Solve Kiya (Step-by-Step Breakdown):**

### **1. Website Behavior Samjhi:**

* Login social media (OAuth) se hota tha.
* `/social-login` automatically login karwata tha.
* Login hone ke baad **nayi session cookie** set hoti thi.
* `change-email` request mein koi CSRF token nahi tha — toh vulnerable tha.

---

### **2. Problem: SameSite=Lax Cookie POST Request Main Nahi Ja Rahi Thi**

Sochna shuru kiya — “Cookie jaise hi refresh hoti hai, woh 2 minute tak active rehti hai.”

---

### **3. Solution: Trick the Browser**

👉 Tumne exploit server main yeh payload banaya:

```html
<form method="POST" action="https://LAB-ID/web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="sda@sadas.com">
</form>
<p>Click anywhere on the page</p>
<script>
    window.onclick = () => {
        window.open('https://LAB-ID/web-security-academy.net/social-login'); // GET request to refresh cookie
        setTimeout(changeEmail, 5000); // Wait 5 seconds, then send POST
    }

    function changeEmail() {
        document.forms[0].submit();
    }
</script>
```

🔑 **Core idea**:

* `window.open()` → browser `/social-login` par jata hai → **cookie refresh hoti hai**
* `setTimeout` → kuch second baad form submit hoti hai → **POST request jati hai with fresh cookie**
* ✅ Email change ho jata hai = attack successful

---

## 💡 **Real-World Application:**

* Jab bhi koi **website OAuth login** use karti ho aur **SameSite cookie policy weak ho**, wahan yeh attack try kar sakte ho.
* **Bina user ke jaane**, uska **email, phone number, shipping address, waghera** badal sakte ho.
* Agar attacker yeh attack kare to uski phishing site pr user sirf ek click karega, aur uski account info silently change ho jayegi.

---

## ✅ Final Summary:

| 🔍 Component         | 💥 Explanation                                                               |
| -------------------- | ---------------------------------------------------------------------------- |
| **Vulnerability**    | CSRF on email change + Weak SameSite=Lax cookie policy                       |
| **Bypass Trick**     | Refresh session via `/social-login` before CSRF attack                       |
| **Payload Type**     | JavaScript + Auto form submission after user click                           |
| **Learning Outcome** | How to bypass browser security using timing, session refresh, and smart flow |
