# 🔁 Horizontal se Vertical Privilege Escalation kaise hoti hai?
Kabhi kabhi horizontal privilege escalation (yaani kisi aur normal user ka access lena) se baat aage barh kar vertical privilege escalation tak pohanch jaati hai — matlab admin jese powerful user ka control mil jata hai.

### 🔓 Example:
Agar attacker kisi aur user ka password reset karne ya uska access le lene mein kaamyaab ho jaye —
Aur wo user agar admin nikla ⛔
Toh attacker directly admin account ka control le leta hai. Yani ab normal user se upar chala gaya — yeh vertical escalation kehlata hai.

## 🎯 Real-world scenario:
Agar attacker URL mein parameter change karta hai, jese:

**https://insecure-website.com/myaccount?id=456**

Aur yeh ID admin user ki hai…

Toh attacker:

Admin ka account page dekh sakta hai.

Wahaan se admin ka password mil sakta hai ya usko change karne ka option ho sakta hai.

Ya phir admin features ka direct access mil sakta hai.

### 🧠 Summary in your words:
Jab attacker kisi normal user ka access le le (horizontal), aur agar usi 
tareeqay se kisi admin user ka access mil jaye to usko vertical escalation bolte hain — yaani neeche se upar tak pohanch jana.

