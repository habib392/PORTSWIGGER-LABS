# Lab: User Role Controlled by Request Parameter (Forgeable Cookie)

### 🎯 Goal:
Admin panel access karna aur `carlos` user ko delete karna.

## 🧪 Step-by-Step Solution (Burp Suite Required)

### 1. 🔐 Login to the Website
- Website open karo
- Login page pe jao
- Use these credentials:
  - **Username:** `wiener`  
  - **Password:** `peter`

### 2. ⚙️ Setup Burp Suite
- Burp Suite open karo
- Jaao: **Proxy → Intercept → ON**
- Phir jaao: **Proxy → Options → Intercept Server Responses**
  - ✅ Is option ko enable karo (tick karo)

### 3. 🛰️ Intercept the Login Request
- Login form fill karo aur **Login button** dabao
- Burp request intercept karega (POST wali)
  - **Forward** kar do
- Ab **GET response** intercept hoga
  - Usme milega:  
    ```
    Set-Cookie: Admin=false
    ```
  - 🛠️ **Isko badal do:**  
    ```
    Set-Cookie: Admin=true
    ```
  - Phir **Forward** kar do

### 4. 🔓 Access Admin Panel
- Ab browser main **Admin panel ka link** show hoga (automatically UI main)
- Click karo "Admin panel" link par

### 5. 🧼 Intercept Admin Panel Request
- Jab admin panel open hoga, Burp GET request capture karega
- Usmein bhi ho sakta hai:
Cookie: Admin=false

- ⚠️ **Isko bhi Admin=true karo**  
- Forward kar do

### 6. ❌ Delete `carlos`
- Admin panel main `carlos` naam ka user dhundo
- Us ke saath **Delete** button par click karo
- Burp 2 alag GET requests capture karega:
1. Pehli request:
   - Admin=false → Admin=true karo
2. Doosri request:
   - Admin=false → Admin=true karo
- Dono requests ko modify karke **Forward** kar do

### ✅ Lab Solved!

### 💡 Real Life Pentesting Tip:
Agar koi site sirf cookie `Admin=true/false` se role decide karti hai, to yeh **security by design flaw** hota hai. Server side par proper role-based access control (RBAC) hona chahiye.
