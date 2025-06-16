# Brute-forcing a stay-logged-in cookie

👤 Sabse pehle wiener:peter credentials se login kiya aur Stay logged in option ko tick kiya.

🛠 BurpSuite ke HTTP History mein gaya aur GET /my-account?id=wiener request dekhi.

🍪 Usme stay-logged-in cookie mili, jo Base64 encoded thi. Uska value tha:
d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw

🧩 Is cookie ko Decoder mein decode kiya aur output mila:
wiener:51dc30ddc473d43a6011e9ebba6ca770

🔍 Is hash ko crackstation.net par paste kiya to pata chala ke yeh MD5 hash hai aur password peter hai.

🚀 Wapas BurpSuite mein aaya, usi request ko Intruder mein bheja. Session ID remove ki aur stay-logged-in cookie ko payload position banaya.

⚙️ Payload processing rules set kiye:

- Hash → MD5
- Add prefix → wiener:
- Encode → Base64

✅ Test run kiya aur response 200 mila — iska matlab technique sahi kaam kar rahi hai.

📋 Lab ki di hui password list copy ki aur Peter ko hata ke us list ko payloads mein daala.

👤 URL mein wiener ko carlos se replace kiya:
GET /my-account?id=carlos

⚠️ Payload prefix ko carlos: banaya (colon zaroor lagana hota hai warna lab fail ho jata).

🚨 Attack run kiya — ek response 200 mila jisme "your username is carlos" likha aaya.

🎉 Lab successfully complete ho gaya!
Matlab maine apne account ki valid cookie se structure samjha, phir same structure use karke Carlos ka account takeover kar liya — pure brute-force se.

💡 What I Learned:
- "Stay logged in" feature agar insecure ho, to it can lead to authentication bypass.
- MD5, Base64 aur predictable patterns brute-force se guess ho sakte hain.
- BurpSuite ka Intruder + Payload Processing powerful combo hai.

