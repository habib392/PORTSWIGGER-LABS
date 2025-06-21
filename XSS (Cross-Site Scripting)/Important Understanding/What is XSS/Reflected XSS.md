# 💥 Reflected XSS — Tumhari Zuban Main

Reflected XSS sabse asaan aur basic type hoti hai XSS ki.
Ye tab hoti hai jab website koi input (jaise URL se koi value) le kar usi waqt usko response mein show kar deti hai — bina verify ya sanitize kiye.

### 🧪 Example samjho:

Tum kisi website ka URL open karte ho:

**https://insecure-website.com/status?message=All+is+well.**

Yani website message=All is well ko directly page pe likh deti hai:

**<p>Status: All is well.</p>**

Lekin ab attacker issi ka faida uthata hai aur malicious code daal deta hai:

**https://insecure-website.com/status?message=<script>alert('XSS')</script>**

Ab jab koi innocent user ye link open karega, uske browser mein attacker ka JavaScript code chalega — aur victim ko pata bhi nahi chalega.

### 🕵️‍♂️ Kya ho sakta hai phir?

- Attacker victim ke cookies chura sakta hai

- Victim ki jagah koi bhi action perform kar sakta hai

- Agar victim logged in hai, toh attacker uska session hijack kar sakta hai

### 🧠 Reflected XSS ki khas baat:

- Script URL se aati hai

- Server bas wohi data wapas bhejta hai

- Sab kuch immediately hota hai

- Database ya permanent storage use nahi hoti
