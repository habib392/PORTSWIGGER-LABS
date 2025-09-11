# 📝 Server-side Template Injection with Information Disclosure via User-supplied Objects

### 🟢 Yeh kis tarah ki technique thi?

Ye **Server-Side Template Injection (SSTI)** technique thi. Matlab user ka input template engine (Django) ke andar bina filter ke inject ho gaya. Phir attacker ne `{% debug %}` use karke framework ke andar ka secret data nikal liya.

---

### 🟢 Iss lab main kya khaas baat thi?

* Normally SSTI labs main attacker code execute karta hai (RCE level).
* Lekin is lab ki khaas baat yeh thi ke attacker ko sirf `{% debug %}` use karke **sensitive objects aur settings** expose ho gaye.
* Sabse dangerous object: `settings.SECRET_KEY` jo pure Django project ka master key hai.

---

### 🟢 Iss lab ke main points kya thay?

1. User-supplied template edit karne ka option tha.
2. Invalid payload daal kar pata chala ke backend Django use ho raha hai.
3. `{% debug %}` tag se debugging info expose ho gayi.
4. Debug output me `settings` object mila.
5. `{{settings.SECRET_KEY}}` se secret key reveal ho gayi.
6. SECRET\_KEY milte hi attacker session hijack aur CSRF bypass kar sakta hai.

---

### 🟢 Developer ko kaunsi ghalti nahi karni chahiye thi?

1. **User-supplied templates** ko directly render nahi karna chahiye tha.
2. **DEBUG mode production** main kabhi bhi enable nahi chhodna chahiye.
3. Sensitive objects (jaise `settings`) ko template ke andar expose nahi karna chahiye.
4. Input validation aur sandboxing implement karni chahiye thi.

---

### 🟢 Iss lab main kon kon sa point weak ya vulnerable tha?

* Template edit feature → direct SSTI vulnerability.
* Django ka `{% debug %}` tag → attacker ne isko abuse kar liya.
* `settings` object ka exposure → direct access to SECRET\_KEY.

---

### 🟢 Kya yeh vulnerability aaj bhi milti hai?

Haan ✅

* Real-world Django, Flask, Jinja2, Twig, Smarty jaise template engines me yeh bug abhi bhi milta hai agar developer careless ho.
* Kai bug bounty reports me aaj bhi SSTI + SECRET\_KEY leaks milte hain.

---

### 🟢 Kya yeh vulnerability developer ki wajah se hoti hai?

Bilkul ✅

* Yeh error hamesha developer ki wajah se hota hai kyunki unhone:

  * DEBUG mode ON rakha
  * User input ko sanitize nahi kiya
  * Sensitive config ko template ke andar expose kar diya

---

## ⚡ Analysis

Har web framework ek **secret key** rakhta hai jo sessions, CSRF aur tokens secure karti hai. Agar developer debugging aur insecure template rendering allow kar de, attacker easily is master key tak pohanch jata hai. Matlab **system ke root password** attacker ke haath lag jata hai.
