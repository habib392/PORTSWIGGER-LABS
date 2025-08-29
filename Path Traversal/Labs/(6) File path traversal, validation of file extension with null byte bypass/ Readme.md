### Lab ka Naam

**File path traversal, validation of file extension with null byte bypass**

---

### A to Z Tariqa

1. **Sabse pehle** lab khol aur koi product image pe jaa jo load ho rahi hai.
   Wahan pe image request chal rahi hogi (example: `/image?filename=abc.png`).

2. **Burp Suite on karo** aur proxy lagao. Phir product image wali request intercept karo.

3. Request ke andar jo `filename=` parameter hai, uska naam note karo.
   Yehi wo jagah hai jahan server file ka naam expect karta hai (jaise `filename=1.png`).

4. Ab tu seedha `/etc/passwd` mangna chahta hai, lekin server check karega ke `.png` extension ho.
   Yahan **null byte bypass** kaam aata hai. Null byte (`%00`) ka matlab hai string ko khatam samajhna.

5. Toh tu file path traversal bhi karega aur null byte bhi use karega. Matlab input aisa hoga:

   * pehle directory traversal (`../../../etc/passwd`)
   * phir null byte (`%00`)
   * phir fake extension (`.png`)

   Server ko lagega file `.png` hai, magar null byte ki wajah se asal mein `.png` ke baad ka ignore ho jayega aur `/etc/passwd` file read ho jayegi.

6. Ab request forward kar de aur response check kar.
   Agar sab sahi kiya hai to response mein tujhe **`/etc/passwd` ka content\`** mil jayega (users list jaisi lines).

7. Lab solve ho gaya ✅

---

### Real World Example

Ye ek classic trick thi jo purani languages (jaise PHP, C libraries) null byte pe string terminate kar deti thi. Is wajah se extension validation fail ho jata tha.

