## Lab: Modifying Serialized Objects

Is lab ko solve karne ke do methods hain:

---

### **1st Method (Browser + Intercept)**

1. Website par login kiya credentials `wiener : peter` se.
2. BurpSuite ka **Intercept mode ON** kiya aur page refresh kiya.
3. Intercepted request `GET /my-account` BurpSuite main aayi jisme `session` cookie show ho rahi thi.
4. Burp ke right side main **Inspector** tab par click kiya → request cookie par click kiya.
5. Cookie ka **serialized version** mila. Wahan `b:0` ko `b:1` se replace kiya.
6. "Apply changes" press kiya aur request forward kar di.
7. Browser main admin panel ka link show ho gaya.
8. Admin panel par click kiya → request phir BurpSuite main aayi.
9. Wahan bhi same kiya → `b:0` ko `b:1` se replace karke forward kar diya.
10. Browser main Carlos ko delete karne ka option aaya. Us par click kiya.
11. Delete request Burp main intercept hui → phir same modification karke forward kiya.
12. Browser main dekhne par LAB SOLVED ✅

---

### **2nd Method (BurpSuite Only)**

1. Login kiya website par `wiener : peter` se.
2. BurpSuite ke **HTTP history** tab main request `GET /my-account?id=wiener` capture hui jisme `session` cookie thi.
3. Is request ko **Repeater** main send kiya.
4. Inspector → cookie decode ki → `b:0` ko `b:1` kiya → Apply changes.
5. Request send ki aur response render kiya → Admin panel show ho gaya.
6. Ab same Repeater request ko modify karke likha:

   ```
   GET /admin HTTP/2
   ```

   Aur request send ki. Carlos delete option response main aa gaya.
7. Phir ussi request ko modify karke likha:

   ```
   GET /admin/delete?username=carlos HTTP/2
   ```

   Aur send kar diya.
8. Response main success aaya aur LAB SOLVED ✅

---

* Real pentesting main second method (pure Burp) zyada useful hota hai kyunki browser restrictions aati hain jaise *"admin interface only allowed"*.

---

**Final Note:**
Yeh lab sikhaata hai ke agar serialized objects ko client-side session cookies main store kiya jaye aur integrity check na ho, toh unko modify karke direct **privilege escalation** possible hai.
