# 🤔 Common Questions about XSS

1. XSS vulnerabilities kitni common hain?
Bhai XSS duniya ki sab se zyada milnay wali web vulnerabilities mein se aik hai. Har teesri website mein XSS mil sakti hai agar proper validation aur sanitization na ho.

2. XSS attacks real world mein kitne hotay hain?
Actual mein XSS attacks itne zyada nahi hote jitne SQLi ya CSRF, lekin agar ho jaayein toh bara damage karte hain — especially phishing, session hijack, ya cookie theft.

3. XSS aur CSRF mein farq kya hai?

XSS = browser ko control lena JavaScript ke through

CSRF = victim se woh kaam karwana jo us ne khud nahi chaha, like auto bank transfer

4. XSS aur SQL Injection mein farq kya hai?

XSS = client-side bug hai (browser par chalti hai), aur dusray users ko target karti hai

SQL Injection = server-side bug hai, aur database ko target karti hai

5. PHP mein XSS ko kaise roka jaaye?
Input filter karo (jo characters chahiyein sirf wohi allow karo), aur output encode karo htmlentities() aur ENT_QUOTES se — taake script tag ya quotes browser mein as text show ho, execute na ho.

6. Java mein XSS kaise prevent karain?
Java mein Google Guava jaisi library use karo output encode karne ke liye, aur input filter karo — JavaScript contexts mein Unicode escape bhi kaam karta hai (\u003cscript\u003e).