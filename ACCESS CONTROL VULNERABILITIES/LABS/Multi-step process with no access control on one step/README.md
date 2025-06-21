# 🔓 Lab: Multi-step process with no access control on one step – Urdu Step-by-Step Guide

## Goal: 
wiener ko admin banana by exploiting multi-step flaw.

### 🧩 Step 1: Admin Login karo

Login with:

- username: administrator  

- password: admin

### 🛠️ Step 2: Admin Panel open karo

Admin panel mein jao

Kisi user ko (e.g. carlos) promote karo

Jab confirm ya promote button dabao, toh Burp Suite mein request ky HTTP history main jaon iss path pr **POST /admin-roles**

### 🧪 Step 3: Us request ko Repeater mein bhejo

**POST /admin-roles HTTP/1.1** 
**Cookie: session=admin-session-id**
**username=carlos&action=upgrade** 

phir logout karo iss ky baad dobara log in karo

**username: wiener**
**password: peter**

### 🍪 Step 5: Cookie uthao

**GET /my-account?id=wiener** iss path sy wiener ki session cookie copy karo

### ✏️ Step 6: Repeater mein wiener ki cookie daalo

Repeater waali request mein:

Cookie: line mein administrator ki jagah wiener ki cookie paste karo

**username=carlos** ko change karo to **username=wiener**

### 🚀 Step 7: Request Send karo
Send the request.

LAB SOLVED

### 💡 Real-World Pen Testing Tip:
Jab bhi multi-step forms hoon (jaisay edit profile, reset password, ya user role change), toh har step pe auth check hona chahiye.
Tum Burp Suite se dekho ke koi step access control ke bina run ho raha hai ya nahi — yehi access control bypass ka moka hota hai.
