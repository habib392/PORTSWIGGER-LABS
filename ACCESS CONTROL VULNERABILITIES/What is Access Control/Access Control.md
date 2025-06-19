# ✅ Access Control hota kya hai?
Jab kisi website ya system ko decide karna ho:

“Kaun kya kar sakta hai?”
ya
“Kisko kya cheez ki permission hai?”

Toh usko hum kehte hain Access Control.

### 🧠 3 Important Concepts:
Concept	Asaan Matlab

- Authentication	Ye check karta hai: "Kya ye banda asal mein wiener hai ya koi aur?"
- Session Management	Ye system ko batata hai: "Wiener abhi bhi login hai ya nahi?"
- Access Control	Ye decide karta hai: "Kya wiener ko yeh action karne ki ijaazat hai?" (jaise admin panel kholna, kisi aur ka data dekhna)

### 🚨 Broken Access Control ka Matlab:
Agar website galti se kisi user ko wo cheez access karne de jo uski permission mein nahi aati,
toh hum kehte hain “Access Control Broken hai”

### 🔓 Example:

Normal user directly /admin page access kar le

Ya user apna ID user_id=1002 se user_id=1001 bana ke kisi aur ka data dekh le
→ Yeh sab critical bugs hote hain

### Privilege Escalation kya hota hai?
Jab ek low-level user (jaise normal member) admin-level actions kar le
→ Usay kehte hain Privilege Escalation

## 2 Types hoti hain:

- Horizontal:	User A ne User B ka data dekh liya
- Vertical:	Normal user ne Admin ka kaam kar liya (e.g. delete user, access panel)

### ⚠️ Kyu dangerous hai?
Access Control ka design human soch pe depend karta hai.
Aur humans se galtiyan hoti hain. Is liye yeh flaws kaafi common hote hain — aur real-world websites mein bohot milte hain.

### ✅ Kaise bachao?
Server-side pe strong access checks rakho

Har action pe verify karo: "Kya is user ko ye kaam allowed hai?"

Role-based checks banao: if user.role != 'admin' → deny

URLs, IDs, and actions sab pe restriction lagao

## 🧪 Pentesting Tip:
Jab bhi tum koi app test karo, toh yeh socho:

"Kya main bina permission ke koi action kar sakta hoon?"
"Kya main kisi aur ka data dekh sakta hoon URL/ID change karke?"
"Kya koi non-admin banda admin ka kaam kar sakta hai?"

# EASYILY UNDERSTANDABLE EXAMPLES
### 🧠 ACCESS CONTROL
“Jaise ek baap apne betay ko decide karke deta hai ke wo kaunsi games khel sakta hai aur kaunsi nahi.”
📌 Yaani system har user ko unki permission ke mutabiq cheezein karne deta hai.

### 🚨 BROKEN ACCESS CONTROL
“Jaise police ghalti se chor ko permission de de ke jail ki chabi le aur aur choriyan kare.”
📌 Yaani system ghalti se unauthorized logon ko access de deta hai — jo nahi dena chahiye tha.

### PRIVILEGE ESCALATION
“Jaise koi ghareeb banda achanak badshah ban jaye aur hukum chalane lage.”
📌 Yaani ek simple user ne admin-level power le li — jo uska haq nahi tha.

