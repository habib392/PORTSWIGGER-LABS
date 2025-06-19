# 🔧 Lab: Unprotected admin functionality with unpredictable URL
**Goal: Hidden admin panel dhoondhna (jo source code ya JavaScript mein disclosed hai), open karna, aur carlos ko delete karna.**

### ✅ Step-by-Step Easy Urdu Version:
Lab open karo

Example:

**https://acbd1234.web-security-academy.net/**

Page ka source code check karo:

Browser mein right click karo → View Page Source

Ya DevTools open karo (press F12), aur Sources tab check karo

Ya Burp Suite mein response dekho

JavaScript file ya <script> tag mein admin panel ka URL likha hota hai.
Jaise kuch aisa:

**var adminPanel = '/1d2x3z-admin-abcd/';**

Ya kabhi kabhi HTML mein likha hota hai:

Admin panel located at **/secret-admin-9a7b/**

Ab tum us URL ko open karo:

**https://acbd1234.web-security-academy.net/1d2x3z-admin-abcd/**

Admin panel khul jaayega → carlos ko delete karo ✅

Lab solved!

### 🔍 Penetration Testing Tip:
Yeh lab sikhata hai ke:

Kabhi kabhi URLs predict nahi hote, lekin source code mein leak ho jaate hain

JavaScript files ya hidden HTML comments mein sensitive info hoti hai

✅ Real-life mein JavaScript ya API responses ko hamesha manual ya Burp se analyse karo, wahan se aise hidden admin panels milte hain.
