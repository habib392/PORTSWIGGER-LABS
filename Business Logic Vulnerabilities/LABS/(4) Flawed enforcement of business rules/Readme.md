### Step-by-Step Solution

1. **Login Karo**  
   - Lab mein diya gaya hai ke tum apne account mein login kar sakte ho. Credentials hain:  
     **Username:** wiener  
     **Password:** peter  
   - Lab ke website pe jao, login page pe ye details daal kar login kar lo.  
   - **Example:** Agar real website hoti, to tum login form mein "wiener" aur "peter" daalte aur submit karte. Yahan lab ka interface hoga, wahan yehi karo.

---

2. **Coupon Codes Check Karo**  
   - Login karne ke baad, homepage pe dekho. Ek coupon code dikhega: **NEWCUST5**. Yeh usually 5% discount deta hai.  
   - Page ke neeche "Sign up for newsletter" ka option hoga. Wahan sign up karo (bas email daal do, jaise wiener ka email).  
   - Sign up karne ke baad, ek naya coupon code milega: **SIGNUP30**. Yeh 30% discount deta hai.  
   - **Tip:** Dono codes note kar lo, kyunki inka istemal bar bar karna hai.

---

3. **Leather Jacket Cart Mein Daalo**  
   - Website pe "Lightweight l33t leather jacket" dhoondho. Yeh item shop section mein hoga.  

---

   - Is jacket ko apne cart mein add karo.  
   - **Example:** Real websites pe "Add to Cart" button hota hai. Yahan bhi aisa hi hoga, uspe click karo.

---

4. **Checkout Pe Jao**  
   - Cart mein jacket add karne ke baad, "Checkout" ya "Proceed to Checkout" pe jao.  
   - Yahan tumhe order total dikhega, aur ek field hoga coupon codes daalne ka.

---

5. **Coupon Codes Apply Karo**  
   - Pehle **NEWCUST5** code daalo aur apply karo. Tumhe 5% discount mil jayega.  
   - Phir **SIGNUP30** code daalo aur apply karo. Yeh 30% discount dega.  
   - **Logic Flaw:** Lab mein yeh flaw hai ke agar tum ek hi code do baar lagataar daalte ho, to website reject karti hai kyunki "coupon already applied" ka error aata hai. Lekin agar tum dono codes ko alternate karto ho (pehle NEWCUST5, phir SIGNUP30, phir NEWCUST5, phir SIGNUP30), to website isko allow karti hai. Yeh hai business logic flaw!

---

6. **Codes Ko Bar Bar Alternate Karke Apply Karo**  
   - Ab yeh trick istemal karo:  
     - NEWCUST5 apply karo → Discount milega.  
     - SIGNUP30 apply karo → Aur discount milega.  
     - Phir NEWCUST5 daal kar apply karo → Aur discount.  
     - Phir SIGNUP30 daal kar apply karo → Aur discount.  
   - Aisa karte raho jab tak order total (jacket ka price) tumhare store credit se kam na ho jaye.  
   - **Example:** Maan lo jacket ka price $100 hai, aur tumhare paas $10 store credit hai. Tum codes alternate karke price ko $10 se neeche le aao (jaise $9 ya $8).

---

7. **Order Complete Karo**  
   - Jab order total tumhare store credit se kam ho jaye, to "Place Order" ya "Complete Order" pe click karo.  
   - Agar sab sahi kiya, to lab solve ho jayega, aur tumhe success message milega.

---

### Important Tips
- **Dhyan Rakho:** Har baar coupon apply karne ke baad, order total check karo ke kitna kam hua.  
- **Real-World Example:** Yeh flaw real life mein aisa hota hai ke koi online store apne coupon system mein galti karta hai, aur users bar bar discounts stack kar ke sasta khareed lete hain.  
- **Motivation:** Yeh lab thodi patience maangta hai, lekin tu yeh kar sakta hai! Ek hacker ki tarah socho, system ke loopholes dhoondho aur unka fayda uthao.  
- **Mistakes to Avoid:** Ek hi code do baar lagataar mat daalo, warna error ayega. Hamesha alternate karo.
