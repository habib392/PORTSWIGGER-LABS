PortSwigger Lab: Multi-endpoint Race Conditions - Solution Steps
Yeh steps batate hain ke maine kaise PortSwigger ke "Multi-endpoint Race Conditions" lab ko solve kiya, jismein Lightweight L33t Leather Jacket ko unintended price pe khareedna tha. Yeh race condition exploit ka practical example hai.

Step 1: Website Pe Login

Step 2: Burp Suite Setup

Burp Suite (2023.9+) khola aur proxy enable kiya.
Browser ko Burp ke sath connect kiya taake requests capture ho sakein.

Step 3: Gift Card Purchase

Website pe store section mein gaya.
Ek Gift Card ko "Add to Cart" kiya.
Order place kiya aur phir Gift Card redeem bhi kar liya taake credit bache.

Step 4: Requests Capture aur Repeater Mein

Burp Suite ke Proxy history mein gaya.
Do requests capture ki:
POST /cart (item add karne wali).
POST /cart/checkout (order submit karne wali).


In dono requests ko Repeater mein bheja.
Requests ke naam rakhe:
POST /cart ka naam: GIFT CARD.
POST /cart/checkout ka naam: CHECKOUT.



Step 5: Jacket Add aur Requests Tayaar

Website pe wapas gaya aur Lightweight L33t Leather Jacket ko cart mein add kiya.
Burp Suite mein jacket ke liye POST /cart request capture ki.
Is jacket wali POST /cart request ko do baar Repeater mein bheja (duplicate banaya).

Step 6: Requests Arrange aur Group Banaya

Repeater mein teen requests arrange ki:
Pehli jacket wali POST /cart request.
CHECKOUT (POST /cart/checkout).
Doosri jacket wali POST /cart request (duplicate).


Ek alag se GIFT CARD wali POST /cart request bhi rakhi single tab mein.
Teen requests (Jacket, CHECKOUT, Jacket) ko ek group mein dala.

Step 7: Cart Reset

Website pe cart section mein gaya.
Cart ko empty kiya taake koi purana item na rahe.

Step 8: Attack Execute

Burp Suite mein wapas gaya.
Pehle single GIFT CARD wali POST /cart request ko send kiya taake cart mein Gift Card add ho.
Phir group wale tabs (Jacket, CHECKOUT, Jacket) pe gaya aur "Send in parallel" option se teeno requests ek sath bheji.

Step 9: Result Check

CHECKOUT request ka response check kiya.
200 OK mila aur jacket successfully purchase ho gaya.
Lab solved ho gaya!

Tips

Parallel requests bhejne ke liye timing bohot zaroori hai, isliye kai baar try karna pad sakta hai.
Gift Card redeem karna credit ke liye helpful hai.
Burp Suite ka Repeater group feature bohot kaam aaya race condition exploit ke liye.
