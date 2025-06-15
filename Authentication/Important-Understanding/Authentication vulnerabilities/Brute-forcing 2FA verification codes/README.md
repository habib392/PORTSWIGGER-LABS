# Brute-forcing 2FA verification codes
Jaise passwords guess karte hain, waise hi 2FA (Two Factor Authentication) codes bhi guess kiye ja sakte hain. Yeh codes aksar sirf 4 ya 6 digits ke hote hain (jaise 123456), is liye agar website ne protection na lagayi ho, to yeh code guess karna bohot asaan ho jata hai.

Kuch websites is problem ko rokne ke liye yeh karti hain ke agar koi 3 ya 5 baar galat code dale to us user ko logout kar deti hain. Lekin yeh trick hackers ke liye koi kaam ki nahi hoti, kyun ke wo automation tools se har step ko fast repeat kar sakte hain.

Burp Suite ka Intruder ya Turbo Intruder jese tools se attacker aise multi-step process (login, 2FA verify) ko automate karke brute-force kar sakta hai.

### 💡 Penetration Testing mein iska use:
Pen testers brute-force ke zariye check karte hain ke koi website 2FA code ke against secure hai ya nahi. Agar website mein rate-limiting, lockout ya monitoring nahi hai, to wo vulnerable hoti hai brute-force attack ke liye.
