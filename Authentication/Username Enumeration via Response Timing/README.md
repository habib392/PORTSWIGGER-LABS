✅ Lab: Username Enumeration via Response Timing
🎯 Goal:
Find a valid username based on response time difference, then use it to brute-force the password.

🛠 Tools Used:
Burp Suite (Intruder + Repeater)
Response time analysis
X-Forwarded-For header spoofing

🔄 Understanding the Lab Step-by-Step:
✅ STEP 1: Try Invalid Login
Burp Suite chalu karo aur lab open karo.
Koi invalid username & password (e.g. user=invalid, pass=1234) daal kar form submit karo.
/login request ko Repeater mein bhejo.

✅ STEP 2: Identify Rate Limiting
Bohat zyada galat login karne par server aapka IP block kar deta hai.
Bypass karne ke liye request mein X-Forwarded-For header add karo (ye server ko naya IP dikhata hai).

✅ STEP 3: Observe Response Time
Ab different usernames ke sath test karo.
Jab username galat hota hai to har response ka time barabar hota hai.
Jab username sahi hota hai, aur password lamba hota hai (e.g. 100 characters), to response time zyada hota hai.

✅ STEP 4: Setup Intruder - Pitchfork Attack
Request ko Intruder mein bhejo.
Attack type select karo: Pitchfork
X-Forwarded-For header aur username field mein payload position lagao.
Password ko 100 random characters ka bana do (just to delay response time).

✅ STEP 5: Configure Payloads
🔹 Position 1 (IP spoofing):
Type: Numbers
Range: 1–100
Step: 1
Max fraction digits: 0
(This will create fake IPs like 1.1.1.1, 2.2.2.2 etc.)
🔹 Position 2 (Username list):
Add possible usernames list here (from wordlist).

✅ STEP 6: Analyze Response Times
Attack complete hone ke baad:
Columns > Enable: Response received, Response completed
Dekho kaunsa response significantly slow tha → wo valid username hai.

✅ STEP 7: Brute-force Password for Valid Username
Nayi Intruder attack banao:
Valid username fix kar do.
X-Forwarded-For header aur password field mein payload position lagao.
Payloads:
Position 1 → Fake IPs (again use numbers 1–100)
Position 2 → Password wordlist

✅ STEP 8: Identify Correct Password
Jab koi response 302 (Redirect) dega, wo correct password hoga.
Ab username + password se login karo → 🎉 Lab solved!

