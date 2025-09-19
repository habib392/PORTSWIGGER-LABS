# Race Condition Vulnerability Lab Notes

## 1. Yeh Kis Tarah Ki Technique Thi?
Yeh technique **race condition exploitation** thi. Race condition tab hoti hai jab do ya zyada processes (jaise HTTP requests) ek hi resource (jaise cart ya database) ko ek saath access karte hain, aur server unko properly handle nahi kar paata. Is lab mein, hum discount code ko multiple times apply karte the by sending parallel requests, jisse server confuse ho jata tha aur discount baar baar lag jata tha.

**Real-World Example**: Jaise Amazon pe ek coupon code ek baar hi apply hona chahiye, lekin agar tu 10 tabs se ek saath apply kare aur server slow ho, toh multiple discounts lag sakte hain. Yeh banking mein bhi hota hai, jahan double withdrawal ho jata hai agar two ATMs ek saath use kiye jaayein.

## 2. Iss Lab Mein Kya Khaas Baat Thi?
Khaas baat yeh thi ke shopping cart ka state **server-side session mein store hota tha**, aur discount apply karne ke beech ek chhota **race window** tha. Jab hum coupon code ke requests ko parallel mein bhejte the (Burp Repeater se), server check karne se pehle multiple discounts apply kar deta tha. Isse expensive item (Leather Jacket) ko sasta ya free mein khareed sakte the.

**Step-by-Step Khaasiyat**:
- **Session Dependency**: Cart user ke session pe based tha, bina session ke empty dikhta tha.
- **Parallel Attacks**: Sequential requests mein sirf ek discount lagta tha, lekin parallel mein multiple.
- **Outcome**: Order total itna kam ho jata tha ke store credit se khareed sakte the.

Yeh lab sikhata hai ke concurrency issues ko ignore nahi karna chahiye, warna hackers exploit kar sakte hain.

## 3. Iss Lab Ke Main Points Kya The?
Main points yeh the (step-by-step summary):
1. **Login aur Flow Study**: Wiener:peter se login karo, cheapest item add karo, endpoints identify karo (POST /cart, POST /cart/coupon, GET /cart).
2. **Restrictions Check**: Discount code do baar apply nahi hota normally ("Coupon already applied").
3. **Session Analysis**: Bina cookie ke cart empty, yani server-side state.
4. **Benchmarking**: Sequential requests mein sirf ek success, parallel mein multiple.
5. **Exploit**: Jacket add karo, parallel coupons apply karo, total kam hone pe purchase.
6. **Tool**: Burp Suite Repeater for parallel requests.

Yeh points sikhate hain ke kaise race conditions ko detect aur exploit karte hain. Real-world mein, yeh e-commerce sites pe common hai jaise Shopify ya WooCommerce.

## 4. Kya Ghalti Nahi Karni Chahiye Thi Developer Ko?
Developer ko yeh ghaltiyan avoid karni chahiye thin:
- **No Concurrency Control**: Database operations pe **locking** ya **transactions** use nahi kiya, jisse multiple requests ek saath process ho gaye.
- **No Rate Limiting**: Requests ko limit nahi kiya, jisse parallel attacks asaan ho gaye.
- **Session State Without Checks**: Session pe depend kiya bina atomic operations ke (jaise discount apply aur check ek hi transaction mein).
- **Input Validation Miss**: Coupon apply endpoint pe no idempotency (repeat requests ko handle nahi).

**Mentor Advice**: Developer ko hamesha assume karna chahiye ke users multiple tabs ya tools use karenge. Use **mutex locks** ya **Redis locks** for critical sections. Real example: PayPal ne ek baar race condition fix ki thi jahan double payments ho rahe the.

## 5. Iss Lab Mein Kon Kon Sa Point Weak Ya Vulnerable Tha?
Weak points yeh the:
1. **Race Window in Coupon Apply**: Discount apply hone aur database update hone ke beech gap, jahan parallel requests slip kar sakti thin.
2. **Server-Side Cart State**: Session-based tha, lekin no proper synchronization for concurrent access.
3. **Endpoint Design**: POST /cart/coupon pe no unique token ya nonce check, jisse replay attacks possible.
4. **No Monitoring**: Server responses consistent nahi thin parallel mein, jo vulnerability dikhaata tha.
5. **Lack of Atomicity**: Operations (check if applied, then apply) atomic nahi thin, yani ek unit mein nahi chalti thin.

Yeh points vulnerable the kyoonke developer ne multi-threaded environment ko consider nahi kiya. Teacher tip: Hamesha test karo high-load conditions mein.

## 6. Kya Iss Tarah Ki Vulnerability Aaj Bhi Milti Hai Aur Kya Yeh Developer Ki Wajah Se Hoti Hai?
Haan, iss tarah ki vulnerability aaj bhi milti hai, especially in web apps, APIs, aur distributed systems mein. Yeh bohot common hai kyoonke modern apps scalable hote hain lekin concurrency ko handle karna mushkil.

**Kyun Milti Hai?**:
- Cloud services (jaise AWS) mein multiple servers pe requests distribute hote hain, jahan sync issues aate hain.
- Real-World Examples: 2023 mein ek banking app mein race condition se users double credits mil rahe the. Uber ya DoorDash jaise apps mein order duplicates ho sakte hain agar parallel bookings.

**Developer Ki Wajah Se Hoti Hai?**: Haan, 100% developer ya architect ki wajah se. Yeh coding error hai, jaise:
- Proper locking nahi lagana (e.g., in Java, no synchronized blocks).
- Database transactions nahi use karna (e.g., in SQL, no BEGIN TRANSACTION).
- Testing mein concurrency scenarios ignore karna.
