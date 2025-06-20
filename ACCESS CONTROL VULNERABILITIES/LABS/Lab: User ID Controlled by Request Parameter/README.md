# 🔐 Lab: User ID Controlled by Request Parameter (IDOR)

### 🧠 Aim

Is lab ka goal hai ke tum carlos user ka API key hasil karo by exploiting an IDOR vulnerability.

### 🧩 Background:

Website mein horizontal privilege escalation ka masla hai. Yani tum apne account ke URL mein id parameter ko change karke doosray user ka data dekh sakte ho.

## ✅ Step-by-Step Solution:

🔑 Step 1: Login to Your Account

Use credentials: wiener:peter

Jao "My Account" page par

🔍 Step 2: Observe the URL

URL kuch aisa hoga:

**https://your-lab-id.web-security-academy.net/my-account?id=wiener**

**id=wiener** yahan tumhara username hai — isko change karna hai

🧪 Step 3: Send Request to Burp Repeater

Send to Repeater

🛠️ Step 4: Modify ID Parameter

Change this:

**GET /my-account?id=wiener HTTP/1.1**

To this:

**GET /my-account?id=carlos HTTP/1.1**

Send request

🔐 Step 5: Get API Key

Response mein tumhein carlos ka API key mil jayega

Phir request waly tab py right click kr ky show response in browser pr click kr ky link copy kr ky paste kar do browser main

Copy that API key and paste it in lab’s solution box

✅ Lab solved!

### ⚠️ Real World Impact:

Agar koi user doosray ka account access kar sakta hai sirf ID change karke, to yeh critical vulnerability hoti hai

Attackers sensitive data (API keys, emails, personal info) nikaal sakte hain

### 🧠 Key Learning:

Always check for user-controlled IDs in URLs

Agar id, user, account, uid jese parameters change karke doosray ka data mil jaye — to yeh IDOR hai

### 🧪 Penetration Testing Tip:

Jab bhi user-related page ho, id= jese parameters ko manipulate karo

Burp Suite ka use karo responses compare karne ke liye

Har baar verify karo ke tumhara session sirf tumhara data dikha raha hai ya doosron ka bhi

### 🏁 Conclusion:

Yeh lab IDOR ka classic example hai. Tum normal user hokar carlos ka sensitive data (API key) access kar lete ho sirf ek parameter badal ke. Yeh vulnerability real websites mein bhi kaafi milti hai!
