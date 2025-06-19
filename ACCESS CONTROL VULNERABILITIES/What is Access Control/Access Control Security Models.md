# 🔐 Access Control Security Models
Access Control Models ka matlab hai rules ka ek set jo decide karta hai ke kis user ko kya cheez access karne ki permission hai.
Yeh models har jagah use hote hain: operating systems, web apps, databases, etc.

### ✅ 1. Programmatic Access Control (Matrix Style)
🧠 Socho ek table bani hui hai database ke andar:

### User	Access
- Habib	/profile, /settings
- Admin	/profile, /settings, /admin

System har baar check karta hai ke "Habib ke paas is page ka access hai ya nahi?"

🔧 Use hota hai: Web apps, APIs, microservices

👥 Support karta hai: Roles, users, workflows

🎯 Highly detailed & flexible (lekin thoda complex bhi ho sakta hai)

### ✅ 2. Discretionary Access Control (DAC)
🧠 Socho har file ya resource ka ek owner hota hai — aur wo decide karta hai ke kis user ko kya access dena hai.

📌 Example:

Habib ne ek Google Doc banaya, aur wo decide karta hai:
“Ali ko view access doon, Bilal ko edit access doon.”

🔐 Owner decide karta hai permissions

🔄 Access delegate bhi kiya ja sakta hai (share)

⚠️ Design complex ho sakta hai agar bohot zyada users ho jaayein

### ✅ 3. Mandatory Access Control (MAC)
🧠 Socho military system jahan sirf central authority decide karti hai ke kisko kya access milay ga.

📌 Example:

Tumhara account sirf Confidential files dekh sakta hai.
Secret ya Top Secret access tum change nahi kar sakte — sirf admin karega.

🔐 User ya owner kuch bhi modify nahi kar sakta

🎖 Zyada tar military ya classified systems mein use hota hai

### ✅ 4. Role-Based Access Control (RBAC)
🧠 Socho har user ko ek ya zyada roles milay hue hain — aur har role ke sath kuch permissions linked hain.

📌 Example:

Habib ka role hai Editor, is liye wo content edit kar sakta hai.
Admin ka role hai Admin, wo sab kuch kar sakta hai.

🔁 User join ya leave kare toh sirf uska role assign ya remove karna hota hai — simple!

🎯 Easy to manage, secure, aur flexible

✅ Most commonly used in modern apps

### 🧠 Final Advice for Pentesting:
**Model	Tum kya check karo**
DAC	Kya koi user kisi resource ka access de sakta hai bina auth ke?

MAC	Kya user kisi higher level resource tak illegal access le sakta hai?

RBAC	Kya user apna role change kar sakta hai? Kya role-based bypass ho raha hai?

Programmatic	Kya access matrix ya DB logic flaw hai? (e.g. ID change kar ke bypass)

### 📌 Summary in Your Style:
Model	Example

Programmatic	Jaise list bani ho ke kon kya kar sakta hai

DAC	Jaise tum Dropbox file kisi friend ko share karo

MAC	Jaise army mein sirf general ko top secret access milta hai

RBAC	Jaise office mein “Manager” aur “Employee” alag roles hote hain, aur unke rights bhi alag
