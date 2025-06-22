# 🛡️ How to find and test for XSS vulnerabilities 

Agar tu XSS vulnerability dhoondhna chahta hai, to 2 tareeqay hain:

🔹 1. Automatically (Asaani Se):

Burp Suite ka web vulnerability scanner use kar — yeh reflected, stored, aur DOM-based XSS bohat fast aur reliable tareeqay se pakar leta hai.

🔹 2. Manually (Khud Haath Se):

🔸 Reflected ya Stored XSS:

Har jagah (form fields, URLs, headers) mein ek unique short input daal (jaise **test1234**)

Phir responses mein check kar ke kahaan yeh value wapas aayi.

Jahan reflection mile, wahan JavaScript payload (jaise <script>alert(1)</script>) try kar — agar chal gaya to XSS mil gaya!

🔸 DOM-based XSS (jo JavaScript ke through hoti hai):

**location.search** ya **#fragment** mein input daal.

Browser ka Developer Tools open kar, aur DOM (HTML tree) mein search kar ke check kar kahaan dikh rahi hai.

Phir context ke hisaab se payload try kar.

### ⚠️ Advanced DOM XSS (jo URL se nahi hoti):

Jaise **document.cookie**, **setTimeout()** ya **eval()** jahan JavaScript direct data handle karti hai.

Yeh detect karna mushkil hai — JavaScript code manually review karna parta hai. Yeh bohat time-consuming hota hai.

Burp Suite ka DOM Invader aur JavaScript analysis tool iss sab ko automate karne mein madad karta hai.