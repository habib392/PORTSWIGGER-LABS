# 🔐 Types of Access Controls
### 1. Vertical Access Control
Matlab: Different user types ko alag alag functionality milti hai.

### 📌 Example:

- Admin: sab users ke account delete/update kar sakta hai

- Normal user: sirf apna account dekh sakta hai, delete nahi kar sakta

💡 Socho ye jaise ek building ho jisme admin upar ke floor pe ja sakta hai, lekin normal log sirf neeche tak ja sakte hain.

### Testing Tip: Check karo kya koi normal user URL ya hidden button se admin wali functionality access kar sakta hai? Agar haan → Vertical Privilege Escalation bug hai 🔓

### 🔁 2. Horizontal Access Control
Matlab: Same level ke users ko sirf apna data access karna chahiye.

### 📌 Example:

- Habib login karta hai aur usay apna bank account dikhta hai

- Lekin agar Habib URL me /account?id=1002 type kare aur Ali ka account open ho jaye — to yeh horizontal access control ka issue hai

💡 Socho jaise har student apni report card dekh sakta hai, lekin kisi aur ki nahi.

### Testing Tip: IDs, usernames, emails, file names change karke check karo ke kya kisi aur ka data milta hai?

### 🔄 3. Context-Dependent Access Control
Matlab: System tumhari current situation ya flow ko dekh ke access deta hai ya rokta hai.

### 📌 Example:

- Tum shopping cart me product daalte ho

- Payment karne ke baad tum cart ko edit nahi kar sakte — system rok dega

💡 Socho jaise train ka ticket check hone ke baad tum seat change nahi kar sakte.

### Testing Tip: Check karo kya koi action galat time pe ho sakta hai? (Jaise: payment ke baad cart modify karna)

## 🧠 Summary Table:
Type	Real-life Example	Bug Test Idea

- Vertical	Admin vs Normal user	Kya user admin ban sakta hai?

- Horizontal	Apna vs doosre ka data	Kya user doosre ka data access kar sakta hai?
  
- Context-based	Cart modify after payment	Kya user wrong order me actions perform kar sakta hai?
