## 🔐 CSRF (Cross-Site Request Forgery)

---

### 📌 CSRF Kya Hota Hai?

CSRF yaani Cross-Site Request Forgery ek web vulnerability hai jisme attacker user ke browser ko use karke koi aisi request bhejta hai jo user ne nahi bheji hoti — lekin phir bhi woh request chal jaati hai, sirf is liye ke user login tha aur browser ne automatically cookies bhej di.

**Simple Lafzon Mein:**

> "Tu login tha bank.com pe, lekin kisi doosri site (attacker.com) pe gaya. Us site ne tere browser se bank.com pe farzi request bheji, aur browser ne session cookie ke sath woh request accept karwa di."

---

### ⚠️ CSRF Ka Nuksaan

* Email address change ho sakta hai
* Password reset ho sakta hai
* Paisa transfer ho sakta hai
* Agar user admin hai to poori site ka control attacker le sakta hai

---

### 🧠 CSRF Hone Ki Conditions (Foundation)

1. **Action available ho** — jaise password/email change, paisa transfer
2. **Session cookie use ho rahi ho** — application bas cookie dekh ke user ko pehchan rahi ho
3. **Request ke parameters predictable hoon** — attacker asani se guess kar sake

---

### 🔓 SameSite Cookie Kya Hoti Hai?

SameSite ek cookie attribute hai jo batata hai ke cookie kab bhejni hai.

* **SameSite=Strict** → Sirf apni site se aayi request par cookie jayegi
* **SameSite=Lax** → GET requests me cookie jayegi, POST me nahi
* **SameSite=None** → Har request me cookie jayegi (⚠️ CSRF vulnerable)

Agar SameSite attribute nahi laga hoga, to browser farzi site se bhi cookie bhej dega.

---

### 🔍 Referer/Origin Header Check Kya Hai?

Jab browser request bhejta hai, to woh batata hai ke yeh request kahan se aayi:

* **Origin** → Sirf domain ([https://site.com](https://site.com))
* **Referer** → Poora URL ([https://evil.com/page.html](https://evil.com/page.html))

Agar application yeh headers check nahi karti, to CSRF request accept ho jaati hai.

---

### 💣 CSRF Attack Example:

```html
<html>
  <body>
    <form action="https://vulnerable-website.com/email/change" method="POST">
      <input type="hidden" name="email" value="pwned@evil-user.net" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```

Victim agar is page ko visit kare aur woh login ho vulnerable site pe, to uska email silently change ho jaayega.

---

### 🖼️ GET Method Wala CSRF:

Agar action GET request se perform ho sakta hai, to attacker sirf yeh line use kare:

```html
<img src="https://vulnerable-website.com/email/change?email=pwned@evil-user.net">
```

Page load hote hi image load hone ke bahane GET request chali jaayegi.

---

### 🚚 CSRF Deliver Kaise Karte Hain?

* Apni website pe form ya image tag chipka do
* Victim ko email, WhatsApp, Facebook ya comment ke zariye wahan bhejo
* Victim jaise hi visit karega, farzi request background me chal jaayegi

---

### 🛠️ Burp Suite Se CSRF PoC Banana:

1. Burp Suite me koi request choose karo
2. Right click → Engagement Tools → Generate CSRF PoC
3. Burp auto HTML form generate karega (cookie ke bina)
4. Us HTML ko apni test site me paste karke test karo

---

### 💡 Summary:

> "CSRF sirf tab kaam karta hai jab browser bina soche samjhe cookies bhej deta hai, aur application bina soche samjhe har request accept kar leti hai."

---

### ✅ Defense Against CSRF:

* CSRF Token ka istemal karo
* SameSite cookies use karo
* Referer/Origin header verify karo
* Sensitive actions pe re-authentication lagao
* GET method se kabhi bhi state change mat karwao

---
