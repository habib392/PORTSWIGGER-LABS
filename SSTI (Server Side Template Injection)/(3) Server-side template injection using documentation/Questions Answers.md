# Questions-Answers

## Question 1
Aik baat too clear hoo gyi ki SSTI Vulnerability templates main hi hoti hai

Yaar yeh jo hum ny iss lab main template edit kiya hai iss tarah edit krna real websites main too nhi milta phir agar yeh vulnerability find krni hue too real websites main ki jagah check karein gyein??

Ab yeh freemarker kya hai or iss JavaScript sy kya relation hai??

Iss poori command ka analysis karo 

<#assign ex="freemarker.template.utility.Execute"?new()> 
${ ex("rm /home/carlos/morale.txt") }

---

## Answer 

### 🔹 1. SSTI sirf templates main hoti hai?

**Server-Side Template Injection (SSTI)** wahi hoti hai jahan backend framework templates use karta hai (Jinja2, Freemarker, Twig, Velocity, Handlebars, etc.).
Template ka kaam hota hai → dynamic data ko HTML main render karna.

👉 Real websites main tu kabhi seedha **template editor** nahi milega jaise lab main mila.
Lekin kahin bhi jahan **user input template ka hissa ban jaye** (jaise custom emails, notifications, content pages, error pages, etc.) wahan SSTI exploit ho sakti hai.

Example:

* Blogging platform jo templates allow kare.
* CMS (Content Management System) jahan theme ya widget customize karne deta hai.
* Misconfigured email system jo user input ko template samajhta hai.

---

### 🔹 2. Freemarker kya hai aur JavaScript ka relation?

* **Freemarker** ek **Java-based template engine** hai (na ki JavaScript ka hissa).
* Ye mostly Java web applications main use hota hai (Spring, Struts, etc.) taake HTML, emails, ya config files dynamically banai jaa sakein.
* Relation: **Zero relation with JavaScript**. Dono alag cheezein hain.

  * JavaScript → Browser side / frontend scripting.
  * Freemarker → Server side template engine (Java ecosystem).

---

### 🔹 3. Iss command ka analysis

```ftl
<#assign ex="freemarker.template.utility.Execute"?new()> 
${ ex("rm /home/carlos/morale.txt") }
```

Step by step:

1. `<#assign ex= ... >`
   → Yeh Freemarker syntax hai jo ek variable banata hai (`ex`).

2. `"freemarker.template.utility.Execute"?new()`
   → `?new()` ek **built-in function** hai jo naye Java objects banata hai.
   → Yahan `freemarker.template.utility.Execute` class ka object create kiya gaya.
   → `Execute` class ka kaam hota hai → system commands run karna.

3. `${ ex("rm /home/carlos/morale.txt") }`
   → `${ ... }` syntax engine ko output render karne ka kehta hai.
   → `ex("command")` ka matlab: abhi banaye gaye `Execute` object ko ek argument do.
   → Is case main argument hai: `rm /home/carlos/morale.txt` (Linux ka delete command).
   → Yeh command server par execute hogi aur file delete ho jayegi.

---

### ⚡ Simple samajh:

Tu basically template engine ko bol raha hai →
“Bhai mujhe ek tool (`Execute`) bana ke do jo system commands chala sakta hai…
ab is tool se ek Linux command chalado → `rm`”

Aur engine bina check kiye maan gaya → RCE mil gayi.

---

## Question 2
Matlab freemarker bhi django, tornado jaisa aik template engine hai iss main bhi vulnerability hoo skti hai or hum iss template engine ko manipulate kr ky hum aik variable bnaty hain phir ius variable main aik object bnaty hain phir apni command jis main carlos ki file delete krni hai woh dety hain phir ius ko execute krny ki bhi command hoti hai 

---

## Answer

### 🔹 Freemarker aur baaki template engines

Bilkul! Freemarker bhi **Django (Python), Tornado (Python), Jinja2 (Python), Twig (PHP), Handlebars (Node.js)** ki tarah ek **template engine** hai.

* Ye sab ka kaam hai: **HTML pages / emails / configs** dynamically generate karna.
* Agar developer careless ho jaye aur user input ko direct template main inject kar de → SSTI vulnerability ho jaati hai.

---

### 🔹 Exploit ka mechanism (jo tu ne bola bilkul sahi hai):

1. **Variable banana:**
   `<#assign ex= ... >`
   – Yahan hum ek variable `ex` banate hain.

2. **Object create karna:**
   `"freemarker.template.utility.Execute"?new()`
   – Ye `Execute` class ka object banata hai, jo system commands run kar sakta hai.

3. **Apni command dalna:**
   `ex("rm /home/carlos/morale.txt")`
   – Apni desired Linux command dete hain (yahaan Carlos ki file delete karne wali).

4. **Execute karna:**
   `${ ... }`
   – Ye ensure karta hai ki command actually run ho aur uska result render ho.

---

### 🔹 Simple analogy samajh:

Jaise tu Django template main likh de `{% debug %}` aur woh backend ka pura context dikha de…
Usi tarah Freemarker main humne bola:

* “Bhai ek tool bana do jo commands chalay”
* “Ab is tool se ye specific command chalado”

Aur server ne bina check kiye ye kaam kar diya → **SSTI → RCE**.

---

### ⚡ Bottom line:

* Freemarker bhi ek template engine hai → agar unsafely use ho to vulnerable ho sakta hai.
* Hum exploit karte hain by: **variable assign → object create → command inject → execution**.
* Yeh process sirf Freemarker ka nahi, har template engine apne syntax main allow karta hai (Jinja2, Twig, etc. ka apna tareeqa hota hai).

