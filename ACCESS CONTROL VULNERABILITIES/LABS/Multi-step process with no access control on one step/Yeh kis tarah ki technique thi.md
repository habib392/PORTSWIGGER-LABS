# 🔍 Yeh kis tarah ki technique thi?

Multi-step process mein sirf kuch steps pe access control lagaya gaya hota hai, lekin kisi ek step pe nahi — aur wahi weak point ban jaata hai.

### Type:
➡️ Broken Access Control (specifically: Insecure Direct Access to Step)

### 👨‍💻 Penetration Tester isko kab use karta hai?
Jab tum kisi multi-step action ka analysis kar rahe ho — jaise:

- User role change

- Order cancel/edit

- Email update

- Password reset

- Payment process

### Tum har step ki HTTP request intercept karo (via Burp), aur dekho:

- Kya sab steps par proper auth checks hain?

- Kya session ya role validate ho raha hai?

- Kya kisi step ko direct run karne se action complete ho jaata hai?

- Agar kisi step pe auth check nahi hai, toh wahan bypass possible hai.

### ✅ Is se tum ne kya seekha?
- Har step important hota hai: Sirf last step pe auth check kaafi nahi hota.

- Burp Repeater powerful tool hai: Tum old request ko modify karke test kar sakte ho.

- Session hijack ka faida: Agar tum session cookie change karo, toh doosre user ke liye action perform ho sakta hai.

- Role-based actions ko test karna zaroori hai: Agar user admin nahi hai phir bhi admin action kar le, toh serious flaw hai.

- Yeh real-world mein dangerous hai: Agar kisi site pe yeh flaw ho toh attacker easily apna role upgrade kar sakta hai.

### 🛡 Real-Life Example:
Bank ka system —
Agar "upgrade to premium account" multi-step process ho, aur koi ek request session check ke bina chal jaaye, toh attacker apna account premium bana sakta hai — without paying.

