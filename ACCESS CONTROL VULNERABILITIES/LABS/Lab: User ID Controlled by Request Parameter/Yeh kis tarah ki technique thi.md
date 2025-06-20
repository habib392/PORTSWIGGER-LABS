# 🔍 Technique Type: IDOR (Insecure Direct Object Reference)

### 🧠 Yeh kya hota hai?

IDOR ek aisi vulnerability hai jahan user-controlled parameters (jaise `id`, `username`, `account`) se system
direct kisi cheez ka access deta hai — **bina verify kiye** ke user uss cheez ka malik hai ya nahi.

Yani backend sirf `id=carlos` dekh kar data de deta hai, yeh check nahi karta ke current session wala user actually carlos hai ya nahi.

## 🌍 Real-World Use (Attack Scenario):

### 📌 Example 1: Social Media

**https://site.com/profile?id=102**

Tum apna profile dekh rahe ho. Agar tum **id=103** likho aur doosray user ka profile khul jaye — to yeh IDOR vulnerability hai.

### 📌 Example 2: Banking App

**GET /account?user=alice**

Agar **user=bob** likhne se Bob ka account statement mil jaye — bohot dangerous hai!

### 📌 Example 3: API Key Leakage

Jese iss lab mein hua — normal user ne sirf id badal ke carlos ka API key nikaal liya. Real world mein yeh API key use karke:

- Unauthorized requests bheje ja sakte hain

- Sensitive data chori ho sakta hai

- System ka full access mil sakta hai

### 🛡️ Real Website Protection Tips:
Never trust user input directly (like ?id=...)

Hamesha backend pe ye check hona chahiye ke:

"Kya yeh user is data ko dekhne ka haq rakhta hai?"

Use access control checks before giving out any user-specific data

### 🔚 Final Thoughts:
IDOR simple lagti hai lekin real world mein isse emails, accounts, addresses, even financial records access kiye ja sakte hain.

Isliye har pentester ko IDOR ke liye URLs aur parameters ko manipulate karna aana chahiye 💻🕵️
