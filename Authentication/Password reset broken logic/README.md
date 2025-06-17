# 🛡️ PortSwigger Lab: Password Reset Broken Logic

## 🔍 Vulnerability Summary

Is lab ki password reset functionality insecure hai. Jab user password reset karta hai, to system reset token ka sahi tarah se validation nahi karta. Hum is logic flaw ka faida utha kar kisi bhi dusre user — **like `carlos`** — ka password reset kar sakte hain.

## 🛠️ How to Solve This Lab

### 1. Apne Account ka Reset Token Dekhna
- "Forgot Password" page par jao.
- Username: `wiener` likho.
- Email client me jao, reset link open karo.
- Koi bhi new password daal do.
- BurpSuite open karo, `HTTP History` me `POST /forgot-password` request dhoondo.
- Us request ko **Send to Repeater** karo.

### 2. Token Validation Test
- Repeater me URL aur body dono me se `temp-forgot-password-token` **remove** karo.
- Request send karo.
- Agar **302 Found (redirect)** response milta hai, to iska matlab:
 **Token properly validate nahi ho raha = logic broken**

## 🔁 Exploitation Steps

### 3. Carlos Ka Password Reset Karna
- Browser me wapas jao → "Forgot Password" → Username: `wiener`
- Email client me reset link open karo.
- Koi bhi password set karo.

- BurpSuite me `POST /forgot-password` request dubara Repeater me send karo.

#### Ab request me yeh changes karo:
- `temp-forgot-password-token` ko **remove** kar do (URL aur body dono se).
- `username=wiener` ko change karo to: `username=carlos`
- `new-password` koi bhi strong password set karo.

- Request send karo.
- Agar **302 Found** aata hai = **Carlos ka password successfully reset ho gaya**.

### 4. Final Step: Carlos ka Login
- Website pe login page pe jao.
- Username: `carlos`
- Password: jo tumne abhi set kiya tha.

🎉 **Login successful = Lab Solved!**

## ✅ Summary

- Website password reset ke waqt token ko validate nahi kar rahi.
- Is logic flaw ki wajah se attacker kisi bhi user ka password change kar sakta hai.

## ✍️ Written by: Habib 🇵🇰  
Learning from: PortSwigger
