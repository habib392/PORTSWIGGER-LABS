# ✅ Lab: Method-based access control can be circumvented (Complete Solution)

### 🧠 Goal:
Tum wiener:peter user ho. Apne aap ko administrator banani hai by bypassing method-based access control.

### 🔓 Step 1: Admin panel ka structure samjho

Login with administrator:admin

Admin panel par jao → kisi user ko promote karne ka option milega (e.g. carlos)

Burpsuite ky HTTP main **POST /admin-roles** request ayi gi 

Yeh request Burp Repeater mein bhej do

### 🔁 Step 2: Normal user session set karo

browser main **my account** main jao phir log out karo phir

Login as wiener:peter

Isky baad HTTP history main request generate hogi **GET /my-account?id=wiener** isky andar jo session cookie hai ius ko copy karna hai

or jo repeater jab main **POST /admin-roles** wali request hai ius ky session cookie sy replace kar doo

Cookie: session=abcd1234567890   ← isko update karo
Request send karo → ❌ Unauthorized aayega

### 🛠️ Step 3: Bypass try karo (⚠️ Real magic starts here)

Change method from POST to POSTX:

**POSTX /admin-roles HTTP/1.1**

📌 Response: “Missing parameter”

🧠 Iska matlab backend ne is request ko process kiya, lekin authorization nahi kiya = **good sign!**

### 🔄 Step 4: Ab method change karo to GET

Right click request in Burp → Change request method → GET select karo

Ab URL mein query string bana do: iss main carlos ko replace kar ky wiener type karna hai

**GET /admin-roles?username=wiener&action=upgrade HTTP/1.1**

**Host: your-lab-id.web-security-academy.net**

**Cookie: session=abcd1234567890**

Send karo

✅ Response: Success! Ab tum administrator ban chukay ho!

➡️ Lab solved!

## 🧠 What You Learned (Real-World Pen Test Tip):

Access control kabhi kabhi HTTP method pe depend karti hai (e.g. only POST allowed)

Lekin backend GET requests ko properly secure nahi karta

Tum method change karke action le sakte ho bina permission ke!

