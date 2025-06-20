# Technique Type:
➡️ Insecure Direct Object Reference (IDOR) via URL parameter
+ Data leakage in Redirect Response

📌 Is attack mein kya ho raha tha?
User ID (id=wiener) URL mein directly control ho raha tha.
— Yeh IDOR vulnerability hoti hai.

Jab attacker id=carlos karta hai, toh website usko login page pe redirect kar deti hai.
Lekin 👇

Redirect ke bawajood response body mein Carlos ki API key leak ho jaati hai.
— Yeh Information Disclosure vulnerability hoti hai.

### 🧠 Real-Life Terms Mein:

IDOR = Jab user ka resource (jaise profile, account, API key) kisi predictable ya editable parameter (jaise ?id=wiener) se access ho jaye.

Redirect Response Leak = Jab sensitive data accidentally us response mein rehta hai jo user ko kisi aur page pe bhejne ke liye banaya gaya ho.

### 💡 Penetration Testing Mein Use:
Jab tum dekho ke koi parameter (jaise id=...) user data control kar raha hai, to test karo:

Kya main kisi aur user ka ID dal kar uska data dekh sakta hoon?

Agar redirect ho raha hai, to kya BurpSuite ke response tab mein koi sensitive info leak ho rahi hai?

