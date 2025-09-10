### 🔹 Step 1: Login

* Username: **content-manager**
* Password: **C0nt3ntM4n4g3r**
  Login ho jao content manager panel main.

---

### 🔹 Step 2: Product Template Edit karna

* Product edit page kholo.
* Wahan tumhain **template edit karne ka option** milega (description, etc.).
* Test karne ke liye `${foobar}` likho aur save karo.

👉 Agar error aaya aur usme **Freemarker** ka zikr ho, matlab engine confirm ho gaya.

---

### 🔹 Step 3: Documentation ka use

* Freemarker docs check karo (jo hint main diya hai).
* Usme likha hai `new()` built-in kaafi dangerous hai, kyunki ye Java objects create karwa sakta hai.

---

### 🔹 Step 4: Payload Banana

Docs aur research page (AlbinoWax ka exploit) se pata chalta hai:

```ftl
<#assign ex="freemarker.template.utility.Execute"?new()> 
${ ex("rm /home/carlos/morale.txt") }
```

* Yahan `<#assign>` ek variable `ex` banata hai.
* `freemarker.template.utility.Execute` ek special class hai jo shell command execute kar sakti hai.
* `ex("rm /home/carlos/morale.txt")` se Carlos ka morale.txt delete ho jayega.

---

### 🔹 Step 5: Inject aur Run

* Template editor main apna pehla test payload (`${foobar}`) delete kar do.
* Upar wala payload paste kar ke save karo.
* Product page dobara view karo → command execute ho jaayegi.

---

### 🔹 Step 6: Verify

Agar payload sahi chala to lab solved ka message aa jaayega. ✅

---

### ⚡ Key Points yaad rakhne layak:

1. Sabse pehle **template engine identify karna** zaroori hota hai (error messages se).
2. Documentation ka use karke pata chala `new()` risky hai.
3. Arbitrary command execution ki power mil gayi, jo RCE ban gayi.
4. Developer ki ghalti → user-controlled template allow karna without sanitization.

---
