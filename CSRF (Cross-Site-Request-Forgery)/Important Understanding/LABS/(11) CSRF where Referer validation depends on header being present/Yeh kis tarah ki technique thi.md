# CSRF Referer Bypass Insight (No-Header FallBack)

Is lab ka maksad tha victim ka email address secretly change karna using CSRF attack. Server `Referer` header check karta tha, lekin agar woh header hi missing ho, to request accept ho jaati thi. Yeh ek insecure fallback hai jiska faida uthaya ja sakta hai.

---

## ⚔️ Technique: Referer Header Wala Bypass

Yeh attack CSRF ka tha — Cross-Site Request Forgery.

Server ye check kar raha tha ke request trusted source se aa rahi hai ya nahi, sirf `Referer` header ko dekh kar. Lekin:

* Agar `Referer` galat ho → request block.
* Agar `Referer` hi nahi ho → request allow.

To humne attack design kiya jahan browser deliberately `Referer` header na bheje. Yeh technique flawed validation ko exploit karti hai.

---

## 🏷️ Meta Tag Kya Hai?

`<meta>` tag browser ko extra info deta hai page ke baare mein — jaise encoding, description, aur referrer policy.

Is case mein humne yeh use kiya:

```html
<meta name="referrer" content="no-referrer">
```

Iska matlab: browser kisi bhi request ke sath `Referer` header **bhejega hi nahi**.

Yeh tag sirf `<head>` tag ke andar use hota hai. Agar `body` ke andar likho to browser ignore kar deta hai.

---

## 💡 Head aur Body ka Farq

* `<head>` mein woh cheezein hoti hain jo page ki **background settings** batati hain: meta tags, CSS links, scripts.
* `<body>` mein woh sab hota hai jo **user ko nazar aata hai**: text, buttons, forms, images.

To `meta` jaise background instructions **sirf head mein hi kaam karti hain**.

---

## 🧠 JavaScript ka Role

Humne exploit HTML mein yeh JavaScript lagayi:

```js
<script>
  document.forms[0].submit();
</script>
```

Iska kaam hai ke page load hote hi form **automatically submit** ho jaye — victim ko kuch click karne ki zarurat na ho.

Agar yeh script na hoti:

* Form page pe open hota
* Victim ko manually submit button click karna padta
* Attack visible ho jata

To JavaScript ne attack ko **invisible aur automatic** banaya.

---

## 🌐 Blogger.com pe Exploit Host Karna

Agar tu chahta hai ke victim pehle **teray blog (e.g., blogspot)** pe aaye aur attack wahi se chale, to yeh steps hain:

1. Blogger dashboard mein jao
2. Naya post banao
3. Editor ko "HTML" mode pe switch karo
4. HTML code paste karo jisme:

   * `<meta name="referrer" content="no-referrer">` ho
   * Form ho jo vulnerable site pe POST request bheje
   * JavaScript ho jo form auto-submit kare
5. Post publish karo
6. Victim ko link bhej do

Jab victim teri site pe aaye ga, uska browser form ko silently submit karega bina `Referer` header ke, aur vulnerable site pe email change ho jaye ga.

---

## 🚫 Real-World Considerations

* Blogger jese platforms kabhi kabhi JavaScript ya meta tags ko block kar dete hain
* Modern browsers CSP (Content Security Policy) ki wajah se auto-submissions block kar sakte hain
* Victim already vulnerable site pe login hona chahiye tabhi request kaam karegi

---

## 📌 Summary

Yeh lab ek simple logic pe based tha: agar `Referer` header nahi bheja jaata, to server request allow kar deta hai. Humne `<meta>` tag se header hatwaya, aur JavaScript se form auto-submit karwaya. Iss tarah humne background mein hi email change kar diya, bina user ko kuch bataye.

Yeh technique penetration testing mein bohot useful hoti hai jab servers weak CSRF protections use karte hain — sirf header-based checks par rely karna insecure hai.
