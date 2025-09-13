# SSTI (Server-Side Template Injection) — Freemarker sandbox bypass

## 1. Ye kis tarah ki technique thi?

Yeh **Server‑Side Template Injection (SSTI)** thi — matlab server par chalne wale template engine (yahan Freemarker) ko hamne aisa input diya jo template engine ne evaluate kar liya. Is lab mein khas baat yeh thi ke Freemarker ke sandbox (jo system access rokta) weak tha, isliye hum template expressions se Java ke objects aur unke methods tak pohanch kar file read kar sake.

**Simple summary (first principles style):** agar template engine **evaluation** karta hai aur koi restriction (sandbox) usko puri tarah block nahi kar raha, to attacker expressions ko chain kar ke server resources (files, network) tak pahunch sakta hai.

---

## 2. Lab ki khaas baat kya thi?

* Engine: **Freemarker** (Java platform). Freemarker me Java object methods accessible hon to attacker reflection / object chaining laga sakta hai.
* Vulnerability: **poorly implemented sandbox** — matlab kuch methods reflection ya code-source tak expose ho rahe thay.
* Target: `my_password.txt` file read karna from `/home/carlos/` by chaining methods starting from `product` object.
* Payload ne Java APIs ko indirect rasta diya (getProtectionDomain → getCodeSource → getLocation → toURI → resolve → toURL → openStream → readAllBytes).

---

## 3. Lab ke main points (A to Z)

1. **Find input point** — product description template editable tha.
2. **Confirm evaluation** — safe probe jaise `${7*7}` ya `${"x"?upper_case}` se check karna.
3. **Type probe / introspection** — `${product.getClass()}` se class name le kar pata karna kitni access hai.
4. **Build method chain** — product.getClass() se starting point le kar aisi methods dhoondna jo file path/URL/stream tak le jayen.
5. **Read bytes** — `.readAllBytes()` se byte array mila, fir Freemarker `?join(" ")` se decimals me convert hua.
6. **Decode** — returned decimals ko ASCII me convert kar ke final password mila.
7. **Submit** — lab me required field me decoded string submit kar ke lab solve kiya.

---

## 4. Kya ghalti nahi karni chahiye thi developer ko? (Remediation / Best Practices)

1. **Sandbox ko sahi se configure karo** — allowlist (safe) tokens ka istemal karo na ke denylist.
2. **Do not render untrusted user input as templates** — agar user input ko render karna zaroori ho to use *escape* karo, ya strict template context me treat karo.
3. **Disable reflection & method access** — agar engine support karta hai to reflection features ko block karo.
4. **Use template engine's built-in safe subset** — jaise simple variable interpolation without method calls.
5. **Input validation & output encoding** — server-side validation aur proper escaping lagao.
6. **Least privilege for runtime** — application user ko minimal filesystem permissions do (app user ko `/home/carlos` jaisa sensitive path na chahiye).
7. **Secrets not stored in web-accessible home** — sensitive files ko protected storage me rakho.
8. **Audit logs & monitoring** — suspicious template expressions ya eval errors ko detect karo.

---

## 5. Is lab me kaun kaun se points weak the (vulnerable spots)?

* **Editable template field (product description)** — attacker input directly template context me gaya.
* **Sandbox incomplete** — engine ne `getClass()` jaise methods allow kar diye.
* **File system access via classloader chain** — class protection domain se code source mil raha tha, jisse file system path resolve kiya gaya.
* **Lack of access controls** — application user probably bohat zyada read permissions rakhta tha.

---

## 6. Kya aaj bhi ye vulnerability milti hai? Kya developer ki wajah se hoti hai?

* **Haan**, aaj bhi mil sakti hai — specially custom CMS, legacy apps, ya jahan developers template engines ko default settings pe chhod dete hain.
* **Ye mostly developer/configuration fault hoti hai**: galat sandbox config, untrusted content ko bina sanitize kiye render karna, excessive permissions. Kabhi kabhi framework defaults insecure hote hain — lekin developer ko documentation padh ke secure options enable karni chahiye.
