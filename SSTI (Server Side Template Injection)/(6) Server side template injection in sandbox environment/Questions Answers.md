# Questions Answers 

Mujy pata hai sandbox firewall ki tarah kaam krta hai yeh cheezon ko protect krt hai ab yahan pr yeh shahid template engine ko protect kr rha hai lekin sandbox jahan bhi hoon iun ko test krna chahie kiun ky yeh bypass hoo skty hain

Acha jaisy iss lab main product edit ka option tha or ius main hum ny malicious payload daal diya SSTI wala too real world main too websites main yeh option nhi hota too wahan phir kaisy yeh detect karein gyein vulnerability

tum ny yeh btahea
${product.getClass()}
Ky agar yeh kaam kare too sandbox weak hai too iss ky kaam krny ki kya nishaani mily gi hamy, kaisa output aye ga

Baki labs main too hum yeh vulnerability iss technique {{ 7*7 }} sy test krty thy lekin iss main commands different kiun hain

Yeh jo payload hai
${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve("/home/carlos/my_password.txt").toURL().openStream().readAllBytes()?join(" ")}
Yeh itna bara or complex kiun hai mera matlab baki labs main too bara simple hota tha payload

---

## Answer

# 1) Sandbox ka role — seedha aur seedha example

* **Sandbox** ek restriction layer hai jo template engine ko system APIs (file I/O, runtime exec, reflection) se **alag** rakhta hai — bilkul firewall ki tarah, lekin application-level.
* Maksad: template mein user input se server ki sensitive cheezen (files, OS commands) expose na hon.
* **Agar sandbox weak ho → attacker template expressions ko Java object graph tak pohoncha sakta hai.**

---

# 2) Real-world: jab “product edit” jaisa input point available na ho — kahan se test karein?

Real websites par editable templates kam milte hain. Dusre common vectors:

1. **User-controlled fields** (comments, profile bio, product reviews) — agar server unko template engine se render karta hai => potential.
2. **Admin panels / CMS** — admins ya content managers se editable HTML/markdown aata hai.
3. **File uploads that get rendered** (e.g. markdown files, templates).
4. **Email templates / notification templates** — agar user input wahan insert hota hai.
5. **API endpoints that return templated data** — JSON ya HTML jo template engine se banta ho.
6. **Stored vs reflected SSTI** — reflected: request mein aate hi render; stored: DB mein store ho kar baad mein render.

**Testing approach:** publicly visible inputs pe small safe probes bhejo (arithmetic, string join). Agar response mein probe evaluated nazar aaye toh deeper testing karna. Hamesha authorized scope mein karo.

---

# 3) Simple probes (safe) vs deeper probes — kyun pehle simple use karen

* **Simple probe:** `${7*7}` ya `{{7*7}}` — agar engine evaluate karta hai to SSTI confirm hoti hai.
* Yeh *safe* aur *noisy-low* test hai: sirf expression evaluate karta hai.
* **Agar yeh kaam kare →** template expressions are evaluated. Next step: type probing (getClass) to see exposure level.

---

# 4) `${product.getClass()}` se kya milega — expected output aur nishaaniyan

* Agar tum template mein `${product.getClass()}` likho aur output render ho jaye, expected output kuch is tarah hoga:
  `class com.example.Product`
  ya
  `class org.someframework.Model$Proxy...`
* **Nishaaniyan (what it tells you):**

  * Template engine reflection access de raha hai.
  * Tum object ka concrete Java class name dekh sakte ho → classloader aur package info mil sakta hai.
  * Agar output “class java.lang.String” ya kisi standard class ka naam aaye to reflection accessible hai.
* **Agar chain methods evaluate ho rahi hain** (e.g. `product.getClass().getName()`), matlab zyada freedom hai.

---

# 5) Kyun kuch labs simple `{{7*7}}` use karte hain aur kuch mein complex payload chahiye?

* **Different template engines** (Freemarker, Jinja2, Twig, Velocity) **different features** expose karte hain. Kuch engines seedha math/string eval allow karte hain (isliye `7*7` kaam karta hai).
* **Sandbox level alag hota hai:**

  * Agar engine **only expression language** allow karta hai (no reflection), simple probes hi possible hain.
  * Agar engine **reflection** (getClass, methods) allow kare to tum Java object graph chain kar sakte ho — phir complex payloads lagte hain.
* **Freemarker** especially Java ke upar run hota hai — file read karne ke liye Java APIs ko chain karna padta hai (URI resolve, toURL, openStream, readAllBytes) — isliye payload lamba hai.

---

# 6) Kyun payload itna lamba aur complex hai? (step-by-step reason)

Payload:
`${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve("/home/carlos/my_password.txt").toURL().openStream().readAllBytes()?join(" ")}`

* Reason breakdown:

  1. `product.getClass()` → root Java object mil gaya.
  2. `.getProtectionDomain().getCodeSource().getLocation()` → jahan se class load hui, ek `URL` milta hai.
  3. `.toURI().resolve("/home/carlos/my_password.txt")` → relative/absolute path ko file URI mein convert.
  4. `.toURL().openStream()` → file ka input stream kholta hai.
  5. `.readAllBytes()` → sare bytes le leta hai (array of ints).
  6. `?join(" ")` → Freemarker syntax to join array into space-separated decimals (website returned bytes as decimals).

* **Kyun lamba?** Kyunki tum directly `Files.readAllBytes(Paths.get(...))` jaise convenient static helper access nahi kar paate unless `forName` ya reflection se class access ho. Is chain mein hum existing accessible methods use karke **filesystem tak indirect rasta** bana rahe hain.

---

# 7) Indicators that sandbox is weak (real signs to look for)

* Template expressions are evaluated at all (`${7*7}` returns `49`).
* You can call methods on objects (`${product.getClass()}` shows class).
* Error messages / stack traces reveal Java class names or method names.
* Template output contains unexpected object dumps like `java.io.FileInputStream@...` or byte arrays.
* Presence of debug pages or verbose logs that leak template details.

---

# 8) Practical detection checklist (penetration testing friendly, safe)

1. **Start safe:** `${7*7}`, `${"a".toUpperCase()}` — observe.
2. **Type probe:** `${product.getClass()}` → get class name.
3. **Method probe:** `${product.getClass().getMethods()?size}` or `${product.getClass().getDeclaredMethods()?size}` (engine permitting).
4. **Resource probe (read-only):** try safe non-sensitive files like `/etc/hostname` or application config if in scope — but always follow lab/authorization rules.
5. **Blind OOB checks:** only with permission — use DNS or HTTP callbacks to detect outbound connections. (Do inside authorized scope.)
6. **Log everything** and **report responsibly** — don't exfiltrate sensitive customer data during testing.

---

# 9) Why labs differ in payload style (summary)

* Engine differences (syntax & features).
* Sandbox configuration (some block reflection, others allow).
* Lab author chooses easiest reliable vector for that engine.
* Real world: you must **probe gradually** — simple → introspect → escalate.

---

# 10) Quick example outputs to expect (so tum samajh jao)

* `${7*7}` → `49`
* `${product.getClass()}` → `class com.example.Product`
* If sandbox blocked, `${product.getClass()}` might render as literal `${product.getClass()}` or give an error like `Expression evaluation disabled` or `Method not found`.
* If reflection allowed but file read blocked, you might get `java.lang.SecurityException` or `AccessDeniedException`.

---

# Where They Exist, How to Find

## 1) User-controlled fields (comments, profile bio, product reviews)

**Kya hota hai:** Users jo kuch likhte/submit karte hain — comments, apni profile ka “bio”, product reviews — yeh fields hotay hain jo site pe dikhte hain. Agar backend yeh values template engine se render karta hai to SSTI possible hai.

**Website pe kahan milega:**

* Blog post ke neeche **comment box**.
* User account → **Edit profile / About me / Bio**.
* Product page → **Leave a review** form.

**Kaise test karo (safe):**

1. Ek simple probe bhejo: `${7*7}` ya `${"test".toUpperCase()}`.
2. Us field ko save karo aur woh page dekho jahan yeh text display hota hai (immediately ya baad mein).
3. Agar page mein `49` ya `TEST` nazar aaye → template evaluated ho rahi hai.

**Nishaaniyan (what to look for):** rendered numbers, unexpected capitalization, HTML showing template syntax evaluated.
**Pentest tip:** Always use low-impact probes first and only in-scope targets. Note where the input is reflected (immediate) or stored.

---

## 2) Admin panels / CMS (content managers)

**Kya hota hai:** Admins ya content managers ke liye dashboard hota hai (CMS like WordPress, custom admin). Wahan editors se HTML/markup/templates update hoti hain — agar ye content template engine ke through render hoti ho to attacker ya malicious admin input SSTI cause kar sakta hai.

**Website pe kahan milega:**

* URLs jaise `/admin`, `/wp-admin`, `/cms`, custom management panels.
* “Templates”, “Theme editor”, “Email templates”, “Page builder” sections.

**Kaise test karo (safe):**

1. Agar tum authorized ho to ek benign probe jaise `${7*7}` kisi template/description mein daalo.
2. Save karke public page check karo — kya woh expression evaluate hua?
3. Logs ya UI error messages mein Java class names dekh kar bhi pata chal sakta hai.

**Nishaaniyan:** editable template editors, raw template textarea, WYSIWYG that allows template tokens.
**Pentest tip:** Admin panels zyada sensitive hotay hain — permission aur disclosure policy check karo. Authorized scope bahut important.

---

## 3) File uploads that get rendered (markdown files, templates)

**Kya hota hai:** Kuch sites users ko markdown ya HTML file upload karne deti hain (e.g., blog posts import, profile README, docs). Agar uploaded file ko server template engine se render karta hai, SSTI ho sakta hai.

**Website pe kahan milega:**

* Blog import, documentation upload, profile README (Git-like sites), “upload content” features.

**Kaise test karo (safe):**

1. Small file with `${7*7}` ya `{{7*7}}` content upload karo.
2. Jo page ya viewer render karta hai, wahan dekhon — evaluation mile to vulnerability.

**Nishaaniyan:** file preview pages, rendered markdown viewers, “raw vs rendered” toggle.
**Pentest tip:** Uploads often pass through sanitizers — test both raw render and processed render. Check content-type handling.

---

## 4) Email templates / notification templates

**Kya hota hai:** Apps user ko jo emails bhejte hain (password reset, welcome, invoice) unke liye templates hote hain. Agar user input (name, bio, comment) directly template tokens mein insert hota ho, SSTI ka risk hota hai.

**Website pe kahan milega:**

* Admin → Email settings, Notification templates.
* Template preview pages (send test email).

**Kaise test karo (safe):**

1. Profile name mein simple probe daalo.
2. Trigger an email (password change, welcome).
3. Check email content (or preview) for evaluated probe.
4. Use internal test addresses (do not send to real users).

**Nishaaniyan:** email preview showing evaluated expressions; admin panels that let you edit templates.
**Pentest tip:** Use test accounts and private inboxes for email testing. Don’t spam real users.

---

## 5) API endpoints that return templated data (JSON/HTML)

**Kya hota hai:** Backend APIs kabhi HTML response ya templated JSON fields return karte hain (e.g., product description rendered server-side). Agar API inputs are used in templates, SSTI ho sakta hai.

**Website pe kahan milega:**

* Browser DevTools → Network tab → XHR / Fetch calls.
* Endpoints like `/api/products/123`, `/api/comments`, `/search?q=...`.

**Kaise test karo (safe):**

1. DevTools se request dekho jahan server response mein HTML ya templated fields hain.
2. Input parameter (like `?q=`) mein `${7*7}` bhejo.
3. Response body mein check karo for evaluated result.

**Nishaaniyan:** response bodies containing `49` or other evaluated tokens; HTML fragments inside JSON.
**Pentest tip:** Use proxy (Burp) to modify requests on the fly. Observe headers like `Content-Type` — templated HTML often uses `text/html`.

---

## 6) Stored vs Reflected SSTI — seedha example aur detection

**Reflected SSTI (seedha):**

* **Kya:** Jo expression tum request mein bhejte ho, wahi turant response mein evaluate ho kar aata hai.
* **Example:** Search box mein `${7*7}` daala aur search results page par turant `49` dekha.
* **Kaise detect:** Send request with probe — check immediate response.

**Stored SSTI (baad mein render):**

* **Kya:** Tumne input database mein store kar diya; woh content kisi aur user page ya future page par render hota hai (comments, profiles).
* **Example:** Comment mein `${7*7}` daala, comment page par jab koi user comment load hota hai tab evaluate hota hai.
* **Kaise detect:** Submit probe, phir view the page where stored content appears (maybe different URL or after refresh).

**Why matter:**

* Stored SSTI zyada dangerous because attacker can persist payload and affect many users later.
* Reflected SSTI easier to spot and exploit for single-user targets.

**Pentest tip:** Always check both. For stored, search the site for where that content appears later and monitor logs or pages.

---

## Short overall checklist (copy-paste for notes)

1. Find input points: comments, profile, reviews, admin editors, upload, email templates, API params.
2. Probe safely with `${7*7}` or `${"X".toUpperCase()}`.
3. Use DevTools / Burp to see where input is reflected (response body / network).
4. If expression evaluated, attempt safe introspection: `${product.getClass()}`.
5. Report with PoC, avoid exfiltrating real sensitive data; follow authorization.

