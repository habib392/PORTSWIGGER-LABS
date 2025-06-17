# Resetting passwords using a URL
Agar koi website insecure hai aur forgot password ka link is tarah ka bana rahi hai:

**http://vulnerable-website.com/reset-password?user=my-username**

To iska matlab:

### 🔥 Attack Possible:
Tum agar is URL mein apna username hata ke kisi aur ka username daal do,
jaise:

**http://vulnerable-website.com/reset-password?user=victim-user**
To agar website mein ko'i security check nahi hai,
to tum direct reset page pe chalay jaoge jahan tum victim-user ka naya password set kar sakte ho — full account takeover!

## 🧠 Real-World Pentest Tip:
Is attack ko kehte hain:

🔍 IDOR (Insecure Direct Object Reference)

Aur agar yeh live site pe mil jaye to ye critical severity ka bug hota hai.
Bug bounty platforms jaisay HackerOne, Bugcrowd pe iski $500+ reward tak milti hai.

### ✅ Tum kya test karo:
- Apna account ka reset URL lo
- Usmein apna username change karo kisi aur ke username se
- Dekho kya naya password set karne ka form mil raha hai
- Agar mil raha hai — screenshot, steps, aur PoC banao

# PART 2
## 🔒 Secure Password Reset ka behtareen tareeqa:
✅ Jab user "Forgot Password" karta hai, to website ek strong, random, aur long token generate karti hai — jaisay:

**https://vulnerable-website.com/reset-password?token=a0ba0d1cb3b63d13822572fcff1a241895d...**

## ✅ Iska faida:
Token guess karna mushkil hota hai (high entropy)

Token mein kisi user ka naam ya info nahi hoti

Jab user is link pe jaye, backend check karta hai:

Kya yeh token valid hai?

Kis user se linked hai?

Token expire ho jata hai kuch time baad

Aur password reset hone ke baad turant delete ho jata hai

## ❌ Problem kaha hoti hai?
Kuch websites bas link check karti hain (GET request),
lekin jab user form submit karta hai (POST request),
tab token ko dobara verify nahi karti!

## Matlab attacker kya kar sakta hai?

Apne account ka reset page kholta hai.

Form mein se token hata deta hai ya kisi aur ka laga deta hai.

Aur dusre user ka password change kar deta hai.

Yeh ek serious logical vulnerability hai!

## 🧠 Penetration Testing Tip:
Jab reset URL mile:

### ✅ Check karo:
- Token strong aur random hai ya nahi?
- Kya token ka andar user ka naam ya info chhupi hui hai?
- Kya token sirf ek dafa use ho raha hai?
- Reset form submit hone par bhi token validate ho raha hai ya nahi?
- Agar nahi ho raha — to tumne ek logic flaw pakar liya, jo bug bounty mein critical level ka hota hai.

  # PART 3
 🔥 Password Reset Poisoning ka Simple Matlab:
Agar website reset email mein jo URL send karti hai wo dynamically generate hota hai (matlab backend user ke request pe URL banata hai),
to attacker us URL ko manipulate kar sakta hai — aur dusre user ka password reset token chura sakta hai.

## 🧠 Real World Example:
Suppose reset link kuch aisa ho:

**https://example.com/reset?token=abc123**
Lekin attacker ne "Host" header manipulate kar ke yeh bana diya:

**https://attacker.com/reset?token=abc123**
Agar server ne bina check kiye yeh URL user ko email mein bhej diya,
to victim jab reset link pe click karega, wo attacker.com pe chala jayega — aur token attacker ke haath lag jaayega.
Attacker phir use token se victim ka password reset kar sakta hai.

🛡️ Penetration Testing Tip:
Is attack ko kehte hain:

Password Reset Poisoning (via Host Header Injection)

### ✅ Check karo:

- Kya reset URL mein attacker Host ya X-Forwarded-Host header se apna domain inject kar sakta hai?
- Kya victim ko wahi manipulated link email mein milta hai?

Agar haan — to yeh critical bug hai, bug bounty pe heavy reward milta hai. 💰



