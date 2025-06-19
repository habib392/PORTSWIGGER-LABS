# 🧪 Lab: User Role Can Be Modified in User Profile

### 🎯 Goal:
Admin panel `/admin` par access hasil karni hai aur user `carlos` ko delete karna hai.

## ✅ Tools Required:
- Burp Suite (Community ya Professional)
- Browser with **FoxyProxy** extension installed and enabled
- Burp Certificate trusted (warna HTTPS kaam nahi karega)

## 🔧 Step-by-Step Lab Solution

### 1. 🟢 FoxyProxy Enable Karo
- Browser mein **FoxyProxy extension enable** karo
- Burp ka proxy set ho aur browser us proxy ko use kare

### 2. 🔐 Login to the Website
- Lab open karo
- Login page par jao
- Use credentials:
  - **Username:** `wiener`
  - **Password:** `peter`

### 3. ⚙️ Burp Suite Setup
- Burp Suite open karo
- **Proxy → Intercept → OFF** kar do (hum sirf HTTP history aur Repeater use karenge)
- Browser mein login hone ke baad **"My Account"** page open karo

### 4. 📨 Update Email Request
- Account page par "Update email" ka option milega
- Wahan koi bhi fake email daal do, jaise:  
test@gmail.com

- Submit/update button dabao

### 5. 📜 Burp HTTP History Check
- Burp Suite mein jao → **HTTP history tab**
- Wahan **`POST /my-account/change-email`** wali request ko dhundo
- Right click karo aur **"Send to Repeater"** par click karo

### 6. 🛠️ Modify the Role ID
- Ab Repeater tab open karo
- Wahan request body mein yeh hoga (example):
```json
{
  "email":"test@gmail.com"
}
Isko modify karo aur roleid add karo:

{
  "email":"rocjas@gmail.com",
  "roleid":2
}
Ab Send button dabao

7. 🔍 Confirm Role ID Change
Response tab mein dekho:

"roleid": 2
Agar yeh dikh raha hai, matlab tum admin ban chukay ho ✅

8. 🔓 Access Admin Panel
Browser mein wapas jao

Page refresh karo

Ab tumhe Admin panel ka option dikhai dega

Click karo /admin par jaake

9. ❌ Delete Carlos
Admin panel mein user list dikhegi

carlos naam ka user dhundo

Uske saath jo Delete button hai uspar click karo

🏁 Lab Solved!

