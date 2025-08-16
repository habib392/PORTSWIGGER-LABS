### **Lab: SSRF with whitelist-based input filter**

1. **Product Page Open Karna**

   * Ek product page pe jaa → "Check stock" button pe click kar.
   * Burp Suite on karo aur request intercept kar ke Repeater me send kar do.

2. **Request Analyze Karna**

   * Request me ek parameter hoga:

     ```
     stockApi=http://stock.weliketoshop.net/product/stock
     ```
   * Yeh server is URL ko call karke stock dikhata hai.

3. **Whitelist Check Samajhna**

   * Jab tum URL `http://127.0.0.1/` likh ke bhejo ge → reject kar dega.
   * Matlab developer ne whitelist lagayi hai → sirf `stock.weliketoshop.net` allow hai.

4. **Bypass Attempt (Embedded Credentials)**

   * URL me ek trick hoti hai:

     ```
     http://username@hostname/
     ```
   * Tum `http://username@stock.weliketoshop.net/` bhejo ge to accept ho jaata hai.
   * Matlab server pehle hostname nikalta hai (weliketoshop.net) → whitelist match hoti hai → request forward ho jaati hai.

5. **Trick with # (Fragment)**

   * Agar tum `http://localhost#@stock.weliketoshop.net/` bhejo ge → normally reject kar dega.
   * Lekin agar `#` ko **double URL encode** karke `%2523` bhejo ge → parsing ka confusion create hota hai.
   * Matlab server sochta hai ke hostname still whitelist hai, lekin actual request `localhost` pe jaati hai.

6. **Final Exploit for Admin Panel**

   * Ab localhost ke admin panel pe request bhej:

     ```
     http://localhost:80%2523@stock.weliketoshop.net/admin
     ```

7. **Carlos Delete Karna**

   * Directly request bhej do:

     ```
     http://localhost:80%2523@stock.weliketoshop.net/admin/delete?username=carlos
     ```
   * Yeh successfully Carlos user ko delete kar dega → Lab Solved ✅

---

Developer ne whitelist lagayi thi, lekin woh bas hostname ke upar thi. Tumne URL parsing ka **logic flaw** exploit kiya, jahan credentials aur encoding ne server ko confuse kar diya. Result: SSRF bypass.
