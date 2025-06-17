# Password Change Functionality
Jab hum password change karte hain, toh aksar system hum se current password aur naya password (2 dafa) maangta hai. Yeh process bilkul waisa hi hota hai jaisa normal login page hota hai — yani username aur password verify hote hain.

## Lekin problem yeh hai:

Agar koi attacker bina login hue kisi aur user ka password change kar sakta hai, toh yeh bohot khatarnak vulnerability hai.

### Example: 
Agar form ke andar "username" ek hidden field (jo screen pe nazar nahi aata) mein diya gaya ho, toh attacker request ko edit karke kisi bhi user ka naam wahan likh sakta hai.

⛔ Iska faida uthake attacker:

Users ke naam guess kar sakta hai (username enumeration)

Passwords ko guess karke repeatedly try kar sakta hai (brute-force attack)


### 🔓 Penetration Testing Tip:
BurpSuite ka use karke check karo:

- Kya password change form bina login ke access hota hai?

- Kya hidden fields mein username diya gaya hai?

- Kya attacker request tamper karke dusre user ka password change kar sakta hai?


Agar haan — toh samajh jao: Account Takeover ka strong chance hai.