# 📝 Lab Notes: Server-Side Template Injection using Documentation (Freemarker)

---

### 🔹 Yeh kis tarah ki technique thi?

Yeh **Server-Side Template Injection (SSTI)** technique thi.
Matlab attacker template engine ka syntax inject karke backend ke andar apna code chalata hai. Iss se normal text render karne ki bajaye, attacker backend ki classes, functions aur system commands tak pohanch jata hai.

---

### 🔹 Iss lab main kya khaas baat thi?

* Is lab ka engine **Freemarker** tha (Java based template engine).
* Humne documentation ke through exploit banaya (AlbinoWax ka payload reference).
* Humne template edit karte hue ek dangerous class (`Execute`) use ki jo shell commands chala sakti thi.
* Final goal tha → **Carlos ki morale.txt file delete karna** aur woh humne command `rm` se kar dikhaya.

---

### 🔹 Iss lab ke main points

1. Pehle test payload se template engine identify kiya (`${foobar}` error).
2. Documentation padh kar samjha ke `?new()` ka use karke Java objects create kiye ja sakte hain.
3. Freemarker ki class `freemarker.template.utility.Execute` mili jo commands chalati hai.
4. Payload banaya:

   ```ftl
   <#assign ex="freemarker.template.utility.Execute"?new()>  
   ${ ex("rm /home/carlos/morale.txt") }
   ```
5. Isse system command execute ho gayi aur file delete ho gayi.

---

### 🔹 Developer ki ghalti kya thi?

1. **User input ko direct template main use karna** → sanitize nahi kiya.
2. **Dangerous built-ins disable nahi kiye** (jaise `?new()` ka use).
3. **Error messages expose karna** jahan se engine ka naam leak ho gaya.

---

### 🔹 Kon kon sa point weak ya vulnerable tha?

* **Template editing feature** → directly accessible tha.
* **Input validation missing thi** → user apna syntax inject kar raha tha.
* **Engine ka dangerous functionality open thi** (`Execute` class ka access).

---

### 🔹 Kya aisi vulnerability aaj bhi milti hai?

Haan, aaj bhi milti hai:

* Old CMS, custom made Java/Python/PHP apps jahan developers user ko customization allow karte hain.
* Jab developers directly user input ko template main inject karte hain (email generators, PDF reports, etc.).

---

### 🔹 Kya yeh vulnerability developer ki wajah se hoti hai?

Bilkul!
Developer ki 2 badi mistakes hoti hain:

1. **User input sanitize na karna.**
2. **Template engine ke dangerous features ko disable na karna.**

---

## ✅ Conclusion

SSTI ka root problem hota hai → **trusting user input inside templates**.
Freemarker, Jinja2, Twig, Velocity sab vulnerable ho sakte hain agar developers careless ho jayein.

👉 Penetration tester ke liye pro tip:
Always test simple payloads (`${7*7}`, `{{7*7}}`) to check for SSTI, phir engine identify karke RCE try karo.

---
