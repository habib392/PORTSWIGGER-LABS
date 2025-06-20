# 🔍 1. Yeh kis tarah ki technique thi?
Yeh technique do vulnerabilities ka mix hai:

### ✅ IDOR (Insecure Direct Object Reference)
Attacker ne id=wiener se id=administrator kiya aur kisi aur user ka data dekh liya.

### ✅ Sensitive Data Exposure / Information Disclosure
Response body mein password field pre-filled tha (value ke andar actual password), jo attacker ne easily dekh liya.

### ⚠️ Overall:

Access Control bhi weak tha (kyunke URL se user change ho gaya).

Aur sensitive info properly protect nahi ki gayi thi (HTML source mein leak ho gayi).

### 💡 2. Ek Penetration Tester iss se kya faida utha sakta hai?
Penetration tester:

IDOR ko exploit karke doosre user ka data access karta hai.

Agar response mein sensitive data (jaise password) mil jaaye, to wo privilege escalate kar sakta hai.

Real-world mein kya ho sakta hai?

Admin ka password mil jaye, to attacker system ka full control le sakta hai.

Kisi company ka confidential data access kar sakta hai.

Backdoor ya malicious code daal sakta hai.

### 🛠️ Pro tip: 
Jab bhi form dikhe jisme password field already filled ho — uska HTML source check karo, usi mein value="actual_password" ho sakti hai.

### ❓ 3. "password ko prefill karte hain (masked field mein)" ka kya matlab hai?
Jab tum kisi website pe jao aur account settings page mein password wala field ho jese:

**<input type="password" value="admin123">**

Toh usmein:

Type="password" hota hai, is wajah se browser usko stars (••••••) mein dikhata hai.

Lekin asli password value="admin123" ke andar hota hai — jo attacker HTML code ya BurpSuite se dekh sakta hai.

Yani:

Masked = aankhon ko nahi dikh raha

Prefilled = already field mein dala hua hai

🎯 Summary in your style:
Developer ne password chhupaya zaroor tha (masked), lekin asal mein wo response ke HTML mein plain likha hua tha. Attacker ne URL ka ID change karke admin ka page khola, aur BurpSuite se password nikal liya. Isse wo admin ban gaya — yeh proper hacking trick hai 😎
