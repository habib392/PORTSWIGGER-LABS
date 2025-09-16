### Race Condition Kya Hai?
- Jab ek website ek hi data ko ek saath do alag alag threads (processes) se handle karti hai, toh yeh dono threads ek doosre se takra sakte hain. Isse application mein galat cheezain ho sakti hain.
- Masal ke liye, soch ke tum ek online store se kuch khareed rahe ho aur ek discount code use kar rahe ho jo sirf ek baar kaam karta hai. Agar tum is code ko ek hi waqt mein do baar use karo, toh website confuse ho sakti hai aur dono requests ko accept kar leti hai. Yeh ek race condition hai.

### Race Window Kya Hai?
- Yeh woh chhota sa time hota hai jab yeh "takrao" ho sakta hai. Masalan, jab website check karti hai ke tumne code use kiya ya nahi, aur uske database ko update karne ke beech ka waqt. Yeh waqt bohot chhota, jaise ek second ka hissa, ho sakta hai.

### Isse Kya Problem Hoti Hai?
- Agar koi attacker is race window ka faida uthaye, toh woh application ke rules ko tod sakta hai. Masalan:
  - Ek discount code ko baar baar use karna.
  - Gift card ko multiple times redeem karna.
  - Account se zyada paisay transfer karna.
  - CAPTCHA ko ek hi baar solve karke baar baar use karna.

### Limit Overrun Race Condition
- Yeh ek common type hai jisme tum application ke kisi limit ko cross kar dete ho. Masalan, ek online store ka discount code jo sirf ek baar use hona chahiye, lekin agar tum do requests ek hi waqt mein bhejo, toh website dono ko accept kar leti hai kyun ke usne pehli request ko database mein update nahi kiya hota jab doosri request aati hai.

### Isko Kaise Pakda Aur Test Kiya Jata Hai?
- **Step 1**: Pehle ek aisa endpoint (website ka feature) dhoondho jo single-use ya limited ho, jaise discount code ya gift card redeem karna.
- **Step 2**: Is endpoint pe ek saath kaafi requests bhejo, yeh dekhne ke liye ke kya limit cross ho sakta hai.
- **Challenge**: Yeh requests ka timing perfect hona chahiye taake race window mein takraayein. Yeh waqt bohot chhota hota hai, kabhi kabhi milliseconds ka.

### Burp Suite Ka Role
- Burp Suite ek tool hai jo testers use karte hain race conditions ko pakadne ke liye. Isme **Burp Repeater** ka use hota hai jisse tum ek hi request ko baar baar, tezi se bhej sakte ho.
- Tum is tool se yeh check kar sakte ho ke kya tum kisi limit ko bypass kar paa rahe ho ya nahi.

### Real-World Example
- Ek online store mein ek code hai jo 500 rupees ka discount deta hai, lekin sirf ek baar. Tum checkout pe jate ho, code daalte ho, aur ek hi waqt mein do alag alag tabs se yeh request bhejte ho. Agar website sahi se check na kare, toh dono requests pass ho sakti hain, aur tumhe 1000 rupees ka discount mil jata hai!

### Yeh Seekhna Kyun Zaroori Hai?
- Race conditions dangerous hote hain kyun ke yeh attackers ko rules todne dete hain. Agar tum developer ya tester ho, toh yeh samajhna zaroori hai ke apki website is tarah ke attacks se safe rahe.
