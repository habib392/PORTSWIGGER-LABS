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

## Short overall checklist
1. Find input points: comments, profile, reviews, admin editors, upload, email templates, API params.
2. Probe safely with `${7*7}` or `${"X".toUpperCase()}`.
3. Use DevTools / Burp to see where input is reflected (response body / network).
4. If expression evaluated, attempt safe introspection: `${product.getClass()}`.
5. Report with PoC, avoid exfiltrating real sensitive data; follow authorization.

