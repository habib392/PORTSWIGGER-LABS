## 🔍 Lab Summary:

Is lab mein aik image upload feature diya gaya tha jo strictly JPG/PNG files hi allow karta hai. Lekin humne ismein ek **race condition vulnerability** exploit kar ke malicious PHP file (web shell) upload karke Carlos ka secret access kiya.

---

## ⚔️ Technique ka Naam:

**Race Condition Exploit in File Upload Validation**

---

## 🤔 Yeh kis tarah ki technique thi?

Yeh aik **timing-based vulnerability** hai jahan attacker aur server ke actions ke darmiyan ka chhota sa waqt (window) exploit kiya jata hai.

Matlab:

* Server ne file upload accept kar li
* Lekin validate baad mein karega (jaise antivirus scan)
* Us beech ke waqt mein attacker file ko run kar leta hai

Isi ko kehte hain **race condition**:
Server delete karne se pehle attacker use kar leta hai.

---

## 🌟 Iss lab mein kya khaas baat thi?

* File type validation properly thi (`only .jpg/.png allowed`)
* File immediately upload ho rahi thi, lekin **malicious hone par delete hone mein thoda waqt lagta tha**
* Yeh delay hi exploit ka mauqa ban gaya
* Burp Suite ka use karke manually parallel requests bhej kar file ko execute kiya gaya

Sabse khaas baat yeh thi ke **Turbo Intruder ke bina bhi** yeh exploit possible tha — sirf Repeater ka smart use kar ke.

---

## 🔑 Key Points / Learnings:

1. **Race Condition** tab hoti hai jab do processes (attacker & server) aik hi cheez ke liye compete karte hain time-wise.
2. File validation agar **delayed ya asynchronous** ho, toh attacker exploit kar sakta hai.
3. Server ne file ko scan karne se pehle accessible banaya → **bada security flaw**
4. Parallel requests (Burp Repeater) ya Turbo Intruder is race ko jeetne ka tareeqa hai.
5. PHP shell upload hui aur GET request se secret file ka content reveal hua.

---

## 📅 Kya yeh vulnerability aaj bhi milti hai?

**Haan, bilkul.** Yeh vulnerability real-world web apps mein abhi bhi milti hai — khas kar:

* Old CMS systems
* Improper antivirus integrations
* Misconfigured async validations
* File upload modules jo **direct disk access** dete hain without checks

---

## 🛠️ Kya yeh developer ki wajah se hoti hai?

**Ji haan!** Mostly yeh vulnerability **developer ki coding mistake** ya **bad logic design** ki wajah se hoti hai:

* File save karna aur validate karna **alag-alag time** pe ho raha hota hai
* Developer ko nahi pata hota ke attacker scan ke pehle hi file access kar sakta hai
* File delete hone ka time delay hona security risk hai

Proper secure coding practices follow ki jaayen to yeh avoid ho sakta hai.

---

## 🧠 Final Thoughts:

Race condition exploits tricky hote hain, lekin agar attacker ko timing ka idea ho aur woh smart tools use kare (jaise Burp Repeater / Turbo Intruder), toh yeh vulnerabilities dangerous ho sakti hain.
