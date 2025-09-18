# Limit overrun race conditions — Lab Notes

**Lab:** Purchase race condition (Lightweight L33t Leather Jacket)

## Lab Solving Steps

1. Website kholi → **My account** par gaya → credentials dali: `wiener` / `peter` → login kia.
2. Home se **Lightweight "l33t" Leather Jacket** dhoondi aur `Add to cart` kiya.
3. Cart par gaya, phir FoxyProxy on ki aur BurpSuite proxy chalaya taake HTTP history capture ho.
4. Coupon code `PROMO20` apply kia — discount confirm hua.
5. Burp mein **Proxy → HTTP history** dekha aur **POST /cart/coupon** request capture ki.
6. Request ko **Send to Repeater** kia.
7. Repeater mein ek **tab group** banaya aur total **32 tabs** (original + duplicates) bana liye (Ctrl+R se duplicates).
8. Website par jaa kar cart se coupon hata diya (taake state reset ho).
9. Burp Repeater mein **Send group → Send in parallel** select kia taake multiple requests ek saath bheje jayen.
10. Responses check kiye: kuch tabs mein “Coupon already applied”, kuch tabs mein “Coupon applied” aaya — iska matlab race condition mil gaya.
11. Browser mein cart refresh kia aur dekha ke discount bohot zyada stack ho chuka — \$1337 jacket ab \$15.40 mein aa rahi thi.
12. Maine jacket purchase kar li — lab solved.

---

## 4. Kya hua (Short explanation)

Server ne coupon application ko atomic nahi banaya tha. Jab multiple requests aik hi waqt aayiin, DB update concurrency handle nahi kar paya aur coupon multiple dafa apply ho gaya — isse price bohot kam ho gaya.
