# Lab: Stored XSS into anchor href attribute with double quotes HTML-encoded

✅ Step-by-Step Manual (No Burp)
Lab open karo, comment form dhoondo.

Comment likho:
**Nice post!**

Name likho:
**xyz**

Email likho: 
**xyz@zz.com**

Website field mein daalo:

```javascript:alert(1)```

Kuch sites pe extra filters hote hain – agar yeh payload work na kare, to next try:

```javascript:prompt(1)```

Comment submit karo.

✅ Agar XSS vulnerable hai to:
Click karte hi alert(1) show ho jaayega – 

### Lab Solved 🎯

---

Basically, yeh aik Stored XSS vulnerability thi jo href attribute ke andar user input inject kar rahi thi. Name, Email aur Comment fields
properly HTML-encoded thi, lekin Website field encode nahi ho rahi thi — yahi vulnerability thi. Humne iss ka faida uthaya aur Website field
mein simple payload javascript:alert(1) daala. Jab user author name pe click karta hai, toh JavaScript execute hoti hai aur popup show ho jaata hai.

Iska matlab hai XSS successfully triggered ho gaya — yaani website vulnerable hai aur XSS confirmed hai.

### 🔐 Lesson: 
Jab bhi user input href, src ya kisi attribute mein inject ho raha ho, toh usay properly encode karna zaroori hai — 
warna attacker javascript: scheme se XSS inject kar sakta hai.



