# 🔍 Yeh kis tarah ki technique thi?
Is lab mein humne use ki:

✅ Session Token Brute-Force via Predictable Cookie
Humne cookie decode ki

Uska structure samjha (username:md5(password))

Fir multiple MD5 hashes generate karke base64 banaye

Aur fir Intruder se brute-force attack chalaaya

Type of attack:
Authentication Bypass using Weak Persistent Token

🌍 Real-world mein yeh kab use hoti hai?
Yeh technique real-world mein tab use hoti hai jab:

1. ✅ Tumhe kisi user ka username mil jaaye
Mostly mil jaata hai via registration, forgot password, blog comments, etc.

2. ✅ Website "Remember Me" ya "Stay logged in" type ka cookie use karti ho
Aur uska structure weak ho (like base64(username:md5(password)))

3. ✅ Tumhare paas password list ho ya wordlist
Jo tum username ke sath combine kar ke guess kar sako

🧠 Real-world example:
Imagine karo:

Ek company ka website hai

Users jab "stay logged in" select karte hain to wo base64(username:md5(password)) cookie bana deta hai

Tum ne ali ka username find kar liya blog se

Ab tum MD5 hashes generate kar ke base64 banaate ho

Cookie inject karke test karte ho

Ek match ho gaya — login ho gaya bina password ke

⚠️ Iska Impact:
Account takeover bina credentials ke

Login limit bypass (kyunki login form use nahi ho raha)

Multi-user hijack agar pattern reusable ho

🛡️ Defense (protection):
Real-world companies should:

Never base session tokens on predictable values

Use strong random tokens (e.g., UUIDs)

Secure cookies with:

HttpOnly

Secure

Expiry time

Validate sessions server-side (not just cookie presence)

📌 Summary in 1 Line:
Jab cookie ka structure weak ho aur tumhare paas username + wordlist ho, tab tum brute-force se login bypass kar sakte ho — bina form fill kiye.
