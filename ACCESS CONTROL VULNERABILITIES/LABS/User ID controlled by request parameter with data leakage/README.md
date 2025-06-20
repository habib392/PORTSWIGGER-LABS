# 🔓 Lab: User ID controlled by request parameter with data leakage in redirect

## 🎯 Goal: Carlos ki API key nikalni hai jo redirect response ki body mein leak ho rahi hai.

### ✅ Step-by-Step Simple Solution:
Login Page Open Karo:

Lab open karo.

Username: wiener

Password: peter

Login ho jao.

URL Manually Change Karo:

Login ke baad browser ke address bar mein URL dekho.

Usmein **id=wiener** hoga.

Usko change karke **id=carlos** kar do.

Enter dabao.

Redirect Hoga Login Page Par:

Tum phir se login page par aa jaoge. Ye normal hai.

BurpSuite → HTTP History Open Karo:

BurpSuite ke Proxy tab mein jao.

HTTP History tab mein request dhoondo jo GET /my-account?id=carlos ho.

Request ko Repeater Mein Bhejo:

Us request par right-click karo → Send to Repeater.

Repeater mein Send dabao.

Response Body Check Karo:

Neeche response mein carlos ki API Key nazar aa jayegi.

Usko copy kar lo.

Lab Complete Karo:

API key ko lab ke solution box mein paste karo.

Submit dabao. ✅

### LAB SOLVED

### ⚔️ Real-World Use in Penetration Testing:
Ye vulnerability ka matlab hai ke redirect ke bawajood sensitive data (jaise API key) response body mein leak ho raha hota hai.

Penetration Testing mein isko Access Control bypass + Info Disclosure ka mix mana jata hai. BurpSuite se asani se pakar mein aa jata hai.
