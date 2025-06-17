 # Email ke through password bhejna
 🔐 Email ke through password bhejna (gandi practice hai)
Agar koi website user ka original password email kar rahi hai, to iska matlab hai woh website bilkul insecure hai.

💡 Secure website kabhi bhi password ko plain form mein store nahi karti, is liye woh kabhi bhi "tumhara current password" bhej hi nahi sakti.

Lekin kuch websites yeh karti hain:

Naya password generate karti hain (random sa)

Usay email kar deti hain user ko

## ⚠️ Is mein kya masla hai?

- Email secure nahi hoti
Email ka data middle mein intercept ho sakta hai (man-in-the-middle attack)

- Email inbox insecure hoti hai

- Log apna email multiple devices pe sync karte hain (phone, tablet, laptop)

- Kahi dafa woh devices secure nahi hote

Agar naya password expire na ho ya user change na kare,
to attacker email access milne par pura account le sakta hai.

Asan Alfaz main Jab hum forget password krty hain too kuch website mujy OTP bhejny ky bajahe real new password bna kr bhej deti hain

## 🧠 Real-world Pentest Tip:
Jab aisi website dekho jo naya password direct email karti ho, to:

### ✅ Check karo:

- Kya woh password expire hota hai?
- Kya woh password forcefully change karne ko kehti hai login ke baad?
- Kya woh password strong hai?

Agar nahi — to ye vulnerability reportable hai, aur tumhara ek strong bug bounty point ban sakta hai!



