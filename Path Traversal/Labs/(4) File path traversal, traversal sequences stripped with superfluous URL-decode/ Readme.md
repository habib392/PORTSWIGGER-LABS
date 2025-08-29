# Lab solve

1. **Maqsad samjho (first principles)**

   * Issue: app user input ko check kar ke traversal sequences (`../`) block karti hai, magar uske baad **URL-decode** kar leti hai. Isliye agar traversal sequence *double-encoded* bheji jaye to bypass ho jata hai. Root cause: canonicalization order galat — decode ke baad validate nahi ho raha.

2. **Tools tayar karo**

   * Burp Suite (Proxy + Intercept + Repeater). Browser proxy Burp par set karo.

3. **Request identify karo**

   * Browser se woh page open karo jo product image fetch karta hai. Burp intercept on karo aur woh HTTP request dhoondo jo `filename` ya similar parameter bhejta hai (image fetch endpoint).

4. **Request capture karo**

   * Burp me us request ko intercept karo aur “Send to Repeater” ya direct modify karne ke liye intercept par hi rok lo.

5. **Exploit payload samjho**

   * Server pe: input pe check → *then* URL-decode → use.
   * Agar hum `..%252f` bhejen to `%25` decode hoke `%` banega, phir second decode se `%2f` → `/` ban jayega. Is tarah blocked traversal sequence server ke validation ke time nazar nahi aata par file read ke time woh actual `../` ban jata hai.

6. **Payload likho (exact)**

   * `..%252f..%252f..%252fetc/passwd`
   * (Ye example 3 level traversal hai; lab ke working directory ke mutabiq level adjust kar sakte ho.)

7. **Request modify kar ke bhejo**

   * Burp Repeater me woh `filename` parameter replace karo:
     `filename=..%252f..%252f..%252fetc/passwd`
   * Send karo (or Forward if intercepting).

8. **Response dekho**

   * Agar sahi hua to HTTP response me `/etc/passwd` ki contents nazar aayengi — username:... etc. Yeh lab solve hone ka sign hai.

9. **Alternative checks (agar fail ho)**

   * Agar 3-level kaam na kare to traversal depth badhao/ghatao (`..%252f` repeated).
   * Try other encodings: `%255c` for backslash on Windows, ya mixed case `%25 32 66` (rare).
   * Check agar app base path alag hai — path relative ho to `../../../` ki required count change ho sakti hai.
