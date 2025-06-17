# Resetting user passwords
Jab koi user apna password bhool jata hai, to usay dobara naya password set karna hota hai — is process ko password reset kehtay hain.
Lekin problem yeh hai ke jab password bhool gaya hota hai, to normal login (username + password) kaam nahi karta, is liye website ko koi alternate tareeqa use karna parta hai jaisay:

- email pe reset link bhejna

- SMS ya OTP bhejna

- ya security questions pochnay

Lekin yeh sab tareeqay dangerous bhi ho saktay hain, agar sahi tareeqay se implement na kiye gaye to attacker kisi aur ka password reset karwa sakta hai.

Is liye password reset system ko bohat secure banaya jata hai, warna attacker bina password ke hi account ka access le sakta hai.

 **Hacker kya kare ga agr security question hai ky what is your favourite pet too phir internet sy all pet list nikal kr brute force kr ky pet list
 ko add kr ky check kre ga ky kon sa pet hoo skta hai**

 ## ⚠️ Real-World Pentesting Tip:
Jab tum kisi website ko pentest kar rahe ho aur security questions dikhte hain, to yeh check karo:

- Kya answers case-sensitive hain ya nahi?
- Kya answer length limited hai?
- Kya multiple guesses allowed hain?
- Kya rate-limiting lagi hai?
- Kya answer encrypt ho kar store hua hai?

Agar in sab mein koi kami ho to vulnerability report banake bug bounty ya report kar sakte ho.
