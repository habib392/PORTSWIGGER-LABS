# Broken access control resulting from URL-matching discrepancies

## 🧠 Aim 

Hum ne yeh seekha ke website ka backend kabhi kabhi soft hota hai aur hum headers, methods, ya URL path ko change karke access control ko bypass kar sakte hain.

## Broken Access Control due to URL Matching

Kabhi kabhi website sirf ek hi tareeqay se access ko block karti hai. 
Lekin agar hum URL ka style thoda change karein — jaise capital letters use karein,
ya slash ya extension lagayein — toh website confuse ho jati hai.

### 🔍 Example 1: Capital Letters

**/admin/deleteUser ← Blocked**

**/ADMIN/DELETEUSER ← Backend ne match kar liya, bypass ho gaya**


➡️ Backend ne capital aur small letters ka farq nahi rakha, lekin access control ne rakha.

### 🔍 Example 2: Extra Extension (Spring Framework)

**/admin/deleteUser ← Real URL**

**/admin/deleteUser.test ← Spring ne allow kar diya, auth check skip ho gaya**

➡️ `useSuffixPatternMatch` setting agar on ho, toh extension ignore ho jata hai.

### 🔍 Example 3: Slash at End

**/admin/deleteUser ← Protected**

**/admin/deleteUser/ ← Bypass ho gaya**

➡️ Extra `/` lagane se access control kaam nahi karta.

## IDOR + Horizontal Privilege Escalation

### 🔐 Simple Definition:

Tum ek normal user ho. Tum apna account dekh sakte ho. Lekin agar tum URL ka parameter
(jaise id) change kar do aur doosray user ka account bhi dekh lo — to yeh IDOR vulnerability hoti hai.

### 🔍 Example:

**https://site.com/myaccount?id=123 ← Apna account**

**https://site.com/myaccount?id=124 ← Doosray ka account mil gaya ❌**

➡️ Server ne check hi nahi kiya ke tum id=124 dekh sakte ho ya nahi.

**Yeh kehlata hai:**
- IDOR (Insecure Direct Object Reference)
- Horizontal Privilege Escalation

## ⚙️ Framework Kya Hota Hai?

Framework ek ready-made system hota hai jisse developers website banate hain.

**Examples:**
- Spring (Java)
- Express (Node.js)
- Laravel (PHP)
- Django (Python)

Frameworks mein kuch default settings hoti hain jo security pe asar daalti hain. Agar developer un settings ko sahi handle na kare, toh access control ka bypass ho sakta hai.

## 💡 What I Learned

- URL ka style (capital, slash, extension) kabhi kabhi security ko bypass kar deta hai.
- Access control sirf frontend pe nahi, backend pe bhi strong hona chahiye.
- IDOR ka matlab hai user IDs ko change karke doosray ka data access kar lena.
- Burp Suite ka use karke aise bypass aur bugs dhoondhna asaan ho jata hai.

## 🧪 Penetration Testing Tips

- `/admin` ko `/ADMIN`, `/admin/`, `/admin.test` jaise try karo.
- `id=123` jese parameters ko badalkar `id=124`, `id=999` test karo.
- Access mil gaya to system vulnerable hai.

## 🏁 Conclusion

Broken access control aur IDOR real world mein dangerous bugs hain. Penetration tester
ko har URL aur parameter ko test karna chahiye in different styles. Kabhi kabhi choti si change se badi vulnerability mil jati hai!
