# AUTHENTICATION SAFE KARNY KA TARIQA

## 🔐 1. User credentials ka khayal rakho

Sabse strong security bhi fail ho jati hai agar attacker ko tumhare user ka username/password mil jaye.

Kabhi bhi login info (jaise username ya password) unencrypted (HTTP) connection se mat bhejo.

Tumne agar HTTPS laga diya hai, to ensure karo ke HTTP request automatically HTTPS pe redirect ho jaye.

## 🕵️ 2. Username/Email leak mat hone do

Website check karo ke kahin username ya email publicly accessible to nahi?
Example: kisi profile page pe ya HTTP response ke andar accidentally dikh raha ho.

### Real-life example:
Socho agar kisi site ka login page HTTP pe chalta hai, to koi attacker us traffic ko Wi-Fi pe sniff karke username/password chura sakta hai. Ya agar kisi user ka email search engine pe dikh raha hai, to attacker usi pe password reset attack kar sakta hai.

# ✅ Username ya email pata chal jana dangerous tab banta hai
jab website khud attacker ko zyada info de rahi ho — jaise:

- “Username not found”

- “Email already exists”

- HTTP connection

- Password reset link without token

- Error messages jo confirm kar dein user exist karta hai

## 🔓 Attacker ka faida kaise uthata hai?

1. Brute Force – Jab username mil gaya, ab wo har password try kar sakta hai.


2. Credential Stuffing – Public leaked data se same email/password try karega.


3. User Enumeration – Error messages se confirm karega kaun kaun registered hai.


4. Password Reset Exploit – Agar system weak hai, token hijack ya direct link se reset karega.

## 💡 Pentesting Tip:

Jab bhi tum authentication ya registration test karo, dekho:

Kya system mujhe user existence confirm kar raha hai?

Kya system same error message deta hai har condition mein?
(Incorrect username ho ya password —