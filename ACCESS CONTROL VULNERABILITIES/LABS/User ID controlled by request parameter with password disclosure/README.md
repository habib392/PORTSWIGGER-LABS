# 🔓 Lab: User ID controlled by request parameter with password disclosure

## 🎯 Goal:
Admin ka password nikalna hai, phir admin ban kar Carlos ko delete karna hai.

### ✅ Step-by-step Simple Solution:

Login karo apne account se:

Username: wiener

Password: peter

Login ho jao.

### Account page open karo:

Jab login ho jao, My Account ya similar page par jao.

URL kuch aisa hoga:

**https://your-lab-id.web-security-academy.net/my-account?id=wiener**

ID change karo **"administrator"** mein:

URL mein id=wiener ko **id=administrator** kar do.

**?id=administrator**

Enter dabao.

BurpSuite mein response check karo:

BurpSuite ka HTTP History ya Proxy tab open karo.

Jo request administrator wali hai usko dekho.

Uske response mein admin ka password show ho raha hoga (masked input field ke value attribute mein).

Admin credentials se login karo:

Username: administrator

Password: (jo abhi mila hai Burp mein)

Admin panel ya user list par jao.

Carlos ko delete karo:

Carlos ka user account dhundo.

Usko delete kar do.

### LAB SOLVED

### ⚔️ Real-world Use in Pentesting:
Kabhi kabhi developer login forms mein password ko prefill karte hain (masked field mein) — lekin uski real value HTML source mein hoti hai, jo ek data leakage hai.

Yeh bug:

IDOR ke through account access deta hai,

Aur response mein sensitive data leak hota hai (admin ka password),

Jisse attacker privilege escalation karke system control le sakta hai.

Bhai is lab se tumhe yeh lesson milta hai ke:

Kabhi bhi HTML response ko ignore mat karo.

Input fields ke value="..." attributes check karo.

Burp ka response tab properly scroll karke dekho — treasures milte hain wahan 😄
