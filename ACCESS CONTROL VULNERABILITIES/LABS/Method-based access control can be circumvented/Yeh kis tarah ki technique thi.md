# 🛠️ Yeh kis tarah ki technique thi? (Simple Explanation)

### 📌 Naam:
HTTP Method Confusion Bypass

### 🔍 Asal Masla Kya Tha?
Server ne sirf POST method par authorization check rakha tha.

Lekin jab tum GET method use karte ho, toh server uss check ko ignore kar deta hai.

Yani:

"Agar POST request karega toh poocha jayega: ‘Tum admin ho?’
Lekin GET se karega toh koi poochta hi nahi!"

### 🧠 Technique Ka Naam:
Method-Based Access Control Bypass

Ya

Insecure HTTP Method Handling

### 🧪 Simple Example:
POST /admin-roles = ❌ Unauthorized (check lagta hai)

POSTX /admin-roles = ⚠️ Missing param (matlab backend ne request li)

**GET /admin-roles?username=wiener&action=upgrade** = ✅ Success (bypass ho gaya)

### 🧠 Tum ne kya seekha?
"Har baar sirf URL dekhna kaafi nahi hota — HTTP method bhi check karo

Kyunki kabhi kabhi GET se woh kaam ho jata hai jo POST se blocked hota hai!"

### 🔓 Real Life Pen Testing Tip:
Jab bhi koi form ya admin action mile jo sirf POST pe chal raha ho

Usse GET, HEAD, OPTIONS ya custom method (e.g. POSTX) mein convert karke test karo

Kai baar server galti se method-based access control laga deta hai — tum bypass kar sakte ho
