# 📝 Basic Server-Side Template Injection (Tornado)

## 🔎 Yeh kis tarah ki technique thi?

Yeh technique **Server-Side Template Injection (SSTI)** thi. Matlab developer ne user ka input bina sanitize kiye directly **template engine (Tornado)** main inject kar diya. Is wajah se attacker ne apna Python code dal kar **system commands run kar diye**.

---

## ⭐ Iss lab main kya khaas baat thi?

* Tornado template use ho raha tha jo Python allow karta hai.
* `{{ }}` ke andar humne expressions run kar diye (jaise `7*7`).
* `{% %}` ke andar humne Python code likh kar **os module import** kiya.
* Phir `os.system('rm /home/carlos/morale.txt')` run kar ke file delete kar di.
  👉 Yani sirf name display functionality ke zariye full **Remote Code Execution** possible ho gayi.

---

## 📌 Iss lab ke main points

1. Parameter `blog-post-author-display` user input le raha tha.
2. Input directly Tornado template main inject ho raha tha.
3. Double curly braces `{{ }}` ne injection allow kiya.
4. `{% import os %}` se Python ka os module load kiya.
5. `os.system()` use karke server pe command chala di.
6. Final goal: `/home/carlos/morale.txt` delete karna ✅

---

## 🚫 Developer ki ghalti kya thi?

* User input ko **directly template engine main inject** kar diya.
* Koi **input validation / sanitization** nahi thi.
* Secure coding practice follow nahi ki.
* Template engine ko unnecessary zyada permissions di hui thi.

---

## ⚠️ Weak / Vulnerable points

* `blog-post-author-display` parameter → direct injection point.
* Tornado template ka unsafe use → attacker ko Python code chalane ka chance mila.
* No output encoding / filtering.

---

## 🔥 Kya aaj bhi yeh vulnerability milti hai?

Haan bhai, bilkul!

* Bohot si web apps abhi bhi Django, Jinja2, Tornado, Smarty, Twig jaise template engines use karti hain.
* Agar developer careless ho to aaj bhi **SSTI → Remote Code Execution** mil sakti hai.
* Yehi wajah hai ke bug bounty programs main SSTI bohot dangerous aur high severity vulnerability mani jaati hai.

---

## 🧑‍💻 Kya yeh vulnerability developer ki wajah se hoti hai?

Haan, 100%.

* Agar developer **user input ko directly evaluate** kare template engine main, to SSTI ho jata hai.
* Agar sanitize kare aur whitelist use kare to yeh issue kabhi na mile.
* Matlab root cause hai **developer ka unsafe coding pattern**.

---

## ✅ Summary (simple words)

* Technique: SSTI (Server-Side Template Injection)
* Khaas baat: Python code + OS commands run karna possible tha
* Main points: `{{ }}` + `{% %}` injection → `os.system()` → file delete
* Developer ki ghalti: input sanitize nahi kiya
* Weak point: `blog-post-author-display` parameter
* Aaj bhi hoti hai: Haan, bug bounty main bhi milti hai
* Cause: Developer ki careless coding

---

⚡ **Pentester Tip:**
Jab bhi tumhe template injection ka shak ho, pehle chhoti expressions test karo (`{{7*7}}`). Agar wo evaluate hota hai, to samajh jao tumhare paas **direct entry point to RCE** hai.
