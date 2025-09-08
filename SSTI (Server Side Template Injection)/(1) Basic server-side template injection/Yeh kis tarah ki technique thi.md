# 📝 Basic Server-Side Template Injection (ERB) – Lab Notes

## 🔹 Yeh kis tarah ki technique thi?

* Yeh lab **Server-Side Template Injection (SSTI)** par based tha.
* Isme user input directly template engine (ERB – Embedded Ruby) mein inject hota tha.
* Attacker arbitrary Ruby code run kar sakta tha aur server ke upar control hasil kar sakta tha.

---

## 🔹 Iss lab main kya khaas baat thi?

* Input parameter (`message`) bina validation ke template ke andar evaluate ho raha tha.
* Humne test payload `<%= 7*7 %>` dala aur output mein `49` mila → matlab code server side pe run hua.
* Ruby ka `system()` function use karke humne OS commands execute kiye.
* Target instruction tha: Carlos ki file `/home/carlos/morale.txt` delete karni, jo successful ho gaya.

---

## 🔹 Iss lab ke main points kya thy?

1. Template engine ERB use ho raha tha.
2. User input direct template mein inject ho raha tha (vulnerability).
3. Test payload `<%= 7*7 %>` se injection confirm hui.
4. RCE (Remote Code Execution) mila Ruby ke `system()` function ke zariye.
5. Final payload se Carlos ki file delete ho gayi.

---

## 🔹 Developer ko kya ghalti nahi karni chahiye thi?

1. User input ko bina sanitize kiye template ke andar directly inject karna.
2. Template engine ko expose karna user ke control ke input ke liye.
3. Secure coding practices aur input validation ka use nahi kiya gaya.
4. Parameterized rendering ka use nahi kiya gaya.

---

## 🔹 Iss lab main kon kon sa point weak ya vulnerable tha?

* **Message parameter** → directly template ke andar inject ho raha tha.
* **Template engine (ERB)** → isne attacker ka input evaluate kar diya bina check kiye.
* **Lack of sanitization / encoding** → user input ko raw evaluate kar diya.

---

## 🔹 Kya yeh vulnerability aaj bhi milti hai?

Haan ✅ yeh aaj bhi real-world applications mein milti hai, specially:

* Jab developers **Django, Jinja2 (Python), ERB (Ruby), Twig (PHP)** ya aur template engines use karte hain.
* Jab input ko directly evaluate kar dete hain bina filter ya escaping ke.

---

## 🔹 Kya yeh vulnerability developer ki wajah se hoti hai?

Bilkul 👍

* Yeh 100% **developer ki galti** hai.
* Developer ne user input ko safe render karne ke bajaye direct evaluate karwa diya.
* Agar woh input ko escape/sanitize karta ya safe methods use karta toh SSTI kabhi possible na hoti.

---

Developers cheezon ko shortcut se karte hain aur template engine pe blindly trust karte hain. Agar tum har cheez ko root tak samjh lo – ke input kahan se aaya, kahan inject ho raha hai – toh tumhe solution bhi clear nazar aata hai.” 🚀
