### A → Z steps

1. **Burp Proxy on karo** aur browser ki traffic intercept mode mein rakho.
2. **Us product page** par jao jahan image load ho rahi hai (lab page). Image request ko Burp Proxy mein intercept karo.
3. Intercept hone par **"Send to Repeater"** karo (ya direct modify karo).
4. Repeater/Intercept mein request mein woh parameter dhoondo jo image ka path/filename leta hai — aksar `filename=` ya `file=` hota hai.
5. Parameter ki value ko replace karo with exact payload:

   ```
   /var/www/images/../../../etc/passwd
   ```
6. Request ko forward/run karo (Repeater mein “Go” ya Proxy se forward).
7. Response check karo — agar successful hai to response mein `/etc/passwd` ka content nazar aa jayega (user accounts lines etc.).

### Kyun yeh kaam karta hai (simple):

Application sirf yeh check karti hai ke path **start** `/var/www/images/` se ho — lekin agar tumne aage `../../../etc/passwd` add kar diya to relative traversal root ke bahar jaa ke `/etc/passwd` read ho sakta hai. Validation sirf prefix check thi — canonicalization nahi hui.

### Agar yeh na chale to try karo:

* Double URL-encode karna (agar server decode pehle karta ho) — lekin is lab mein upar wala payload sahi hai.
* Alag starting folder values agar lab hint karein.

### Pentesting tip (tumhare learning ke liye):

* Burp Repeater se pehle **Proxy → Intercept** se request samjho, phir Repeater mein iterate karo.
* Response headers aur status codes check karo (200 ya 403). Error messages se clues milte hain.
* Hamesha payloads small rakh ke test karo aur logs dekho.

### Defence / Mitigation (jo devs ko batao):

1. **Never trust user input** — avoid passing raw path to filesystem.
2. **Canonicalize & realpath** kar ke check karo ke final resolved path root directory ke andar hi ho (e.g., `realpath()` and ensure it starts with allowed base).
3. **Use allowlist** of filenames rather than accepting arbitrary paths.
4. **Avoid exposing full filesystem paths** in parameters; use IDs mapped to safe filenames.
5. Proper **file permissions** so web user can't read sensitive files like `/etc/passwd` (least privilege).
