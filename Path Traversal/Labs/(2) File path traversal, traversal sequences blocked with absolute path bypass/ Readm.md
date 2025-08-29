## File path traversal, traversal sequences blocked with absolute path bypass


### Step-by-step (short & practical)

1. **Goal samjho (root):**
   App filename ko sanitize karnay ki koshish karti hai (traversal sequences block). Lekin woh filename ko *default working directory* ke relative assume karti hai. Iska matlab: agar tum absolute path (starting `/`) bhejo — filter miss kar sakta hai.

2. **Setup:**

   * Browser + Burp Suite proxy on.
   * Intercept ON.

3. **Recon — request dhundho:**

   * Site pe koi product image ya image endpoint open karo (product page).
   * Burp me woh request intercept karo jo image fetch karta hai (usually `GET /image?filename=...` ya similar).
   * Note karo parameter name (example: `filename`, `file`, `img`, `image`).

4. **Test basic traversal (confirm behavior):**

   * Pehle try: `filename=../../../etc/passwd` — agar response block ho ya sanitized ho to samjho traversal sequences blocked hain. (Confirm karey ke woh sanitize kar rahi hai.)

5. **Absolute path bypass (main trick):**

   * Modify request so that `filename=/etc/passwd` (leading slash, absolute path).
   * Kyunki app "relative to default working directory" treat karti hai, absolute path bypass karegi.

   **Example original request (intercepted):**

   ```
   GET /product/image?filename=product123.jpg HTTP/1.1
   Host: vulnerable.lab
   User-Agent: ...
   ```

   **Modify to:**

   ```
   GET /product/image?filename=/etc/passwd HTTP/1.1
   Host: vulnerable.lab
   ```

6. **Send & observe:**

   * Forward the intercepted request (or send via Burp Repeater).
   * Agar vulnerable, response body will contain contents of `/etc/passwd` (text with lines like `root:x:0:0:root:/root:/bin/bash`).

7. **If direct absolute path is blocked — fallback tricks:**

   * URL-encode the slash or parts: `%2Fetc%2Fpasswd` → `filename=%2Fetc%2Fpasswd`
   * Try `//etc/passwd` (double slash) — sometimes works.
   * Try `/.%2e/etc/passwd` or other encodings.
     (Note: bahut advanced evasion techniques hain — lab usually accepts plain `/etc/passwd`.)

8. **Use Burp Repeater for iteration:**

   * Repeater se rapidly edit/send karo, headers dekho, responses compare karo.
   * Check response length and Content-Type (`text/plain` suggests file content).

9. **Proof & finish:**

   * Jab response `/etc/passwd` show karey — lab solved.
   * Take note: capture screenshot only if lab allows; otherwise save response content as proof per lab rules.

10. **Mitigation (developer perspective / report):**

    * Don’t pass user input directly to filesystem APIs.
    * Use allowlist of filenames or map user input to safe internal identifiers.
    * Canonicalize and verify paths, and enforce that resolved path is inside allowed directory (compare realpath).
    * Run app with least privilege; don’t expose sensitive dirs.

# Quick checklist (for you)

* [ ] Intercept request that returns an image.
* [ ] Identify parameter name.
* [ ] Try `../../../etc/passwd` (confirm traversal blocked).
* [ ] Try `filename=/etc/passwd`.
* [ ] If success → you have file contents → lab solved.
* [ ] If fail → try URL-encoding / other encodings.
* [ ] Always use Burp Repeater to test fast.

