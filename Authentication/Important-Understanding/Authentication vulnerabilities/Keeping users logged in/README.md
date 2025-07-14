# Keeping users logged in
"User ko login rehnay dena (Remember me ka system)"

Bohat si websites pe ek checkbox hota hai “Remember me” ya “Keep me logged in”, jo user ko bar bar login karne se bachata hai. Yani agar browser band bhi kar dein to bhi login rehta hai.

Is feature ke liye website ek token banati hai (jise "remember me cookie" kehte hain) aur user ke browser mein store kar deti hai. Agar kisi ke paas yeh cookie aa jaye to wo bina password dale login ho sakta hai. Is liye yeh cookie secure aur guess nahi hone wali honi chahiye.

Lekin kuch websites galti yeh karti hain ke wo yeh cookie simple tareeqay se banati hain, jaise:

Username + timestamp ka simple combo

Kabhi kabhi password bhi use karti hain (jo bohat risky hai)

Agar attacker apna khud ka account bana le, to wo apni cookie dekh ke formula samajh sakta hai ke cookie kaise banti hai. Phir wo brute-force karke doosray users ki cookies guess karne ki koshish karta hai — bina login limits ke.

Kuch websites sochti hain ke agar cookie Base64 jese encoding se "encrypt" kar dein to safe ho jayegi. Lekin bhai Base64 encryption nahi hota, sirf encode karta hai — aur yeh koi bhi decode kar sakta hai.
Agar website ne hashing lagayi hai, lekin salt nahi diya to attacker phir bhi wordlist hash karke cookies guess kar sakta hai.

Aur agar attacker khud account nahi bhi bana sakta, to:

Wo XSS se kisi aur user ki cookie churakar analysis kar sakta hai.

Agar website kisi open-source CMS pe bani hai (jaise WordPress, Laravel), to cookie ka structure public docs mein mil jata hai.

### 💡 Penetration Testing mein iska use:
Pen tester ya attacker:

Apni cookie analyze karta hai.

Dekhta hai cookie ka pattern.

Brute-force karta hai ya XSS use karke doosron ki cookies churata hai.

Login attempt limit ko bypass kar leta hai cookie-based attack se.

Yani remember me feature agar theek se implement na ho, to attacker asani se login process ko hi skip kar sakta hai.

#### Kabhi kabhi websites cookie mein user ka password ka hash daal deti hain (jaise MD5 hash), lekin agar wo password koi common password ho (jaise 123456, password, qwerty), to uska hash already internet pe available hota hai.

To attacker ko sirf wo hash chahiye hota hai. Wo us hash ko crackstation.net jese tools mein ya Google mein paste karta hai, aur agar match mil gaya — to attacker ko original password mil jata hai, bina kuch complex cracking ke.

## Yeh kyun dangerous hai?
Kyunki:

Agar password weak ho to uska hash ka koi faida nahi.

Har user ka password same hashing algorithm (e.g. MD5) se bana ho to sabka guess lagana asaan ho jata hai.

## 🧂 Salt ka kya role hai?
Salt ka matlab hota hai — har user ke password ke sath ek random cheez mila do (jaise password123 + Xz#7!) phir uska hash lo.

Isse har user ka hash unique ban jata hai, aur precomputed hash lists (jese crackstation) se match nahi hota.


