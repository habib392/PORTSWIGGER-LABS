# Two-Factor Authentication Tokens kya hote hain?
Yeh wo codes hote hain jo login ke waqt verify karne ke liye use kiye jaate hain — usually yeh code user ke paas kisi device se aata hai.

**2 tareeqay hotay hain code dene ke:**
- 1. Secure Devices ya Apps:
Kuch websites tumhein ek special device deti hain (jaise RSA token ya keypad) — jo code khud generate karta hai.

Ya phir tum Google Authenticator jaisi app use karte ho, jo har 30 second mein naya code banati hai.

Yeh tareeqa secure hota hai, kyunke code sirf tumhare device par banta hai, kisi aur jagah se send nahi hota.

- 2. SMS ke zariye code bhejna:
Kai websites tumhare mobile pe text message (SMS) ke through verification code bhejti hain.

Yeh tareeqa kam secure hota hai.

## SMS codes ke issues kya hain?
Intercept ho sakta hai — SMS network secure nahi hota, attacker beech mein code chura sakta hai.

### SIM swapping attack:
Attacker tumhari SIM ki duplicate copy le leta hai (fraud se).
Phir uske paas tumhare sab SMS aane lagte hain, including OTP codes.
Is tarah attacker bina tumhare phone ke bhi login kar sakta hai.

### ✅ Secure OTPs:

Jo Google Authenticator, Authy, ya kisi app se aate hain

Ya kisi hardware token (RSA device) se generate hote hain

Yeh tumhare device ke andar hi bante hain, kahin se bheje nahi jaate

Isliye churaana mushkil hota hai, isay secure maana jaata hai

### ❌ Kam secure OTPs:

Jo SMS se aate hain

Ya email ke zariye milte hain (agar email protect nahi hai)

Inhe beech mein intercept kiya ja sakta hai

SIM swapping ya email hack se bhi attacker OTP le sakta hai

💡 WhatsApp se aane wale OTPs bhi tabhi secure hain jab WhatsApp account secure ho (2FA laga ho, phone lock ho, etc.)

## To best practice:
✔ Google Authenticator type app use karo
✔ SMS OTP pe rely mat karo jab tak aur option na ho
✔ Email pe 2FA laga ke rakhna zaroori hai



