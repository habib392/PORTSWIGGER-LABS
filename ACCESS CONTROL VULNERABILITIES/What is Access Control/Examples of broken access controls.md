# Broken Access Control
Jab ek user aisa resource ya kaam access kar le jo uski permission mein nahi aata, toh usay kehte hain Broken Access Control

### Vertical Privilege Escalation – Admin banne wali galti
📌 Example:

Habib ek normal user hai

Lekin agar wo directly URL me /admin/delete-user?id=1002 likh ke kisi ka account delete kar le
👉 To yeh vertical privilege escalation hai (user → admin level ka kaam)

🔐 System ne check hi nahi kiya ke "Habib admin hai ya nahi"

### 🔓 Unprotected Functionality – Jab sensitive pages open ho jayein bina permission ke
📌 Example:

Website ka ek hidden admin panel hai:

**https://insecure-website.com/admin**

Admin users ko toh iska link milta hai dashboard pe...

Lekin agar Habib jaise normal user ko bhi ye URL mil jaye ya manually type kar le — aur wo open ho jaye
👉 Toh system ki security broken hai!

📁 Kahan se milta hai hidden URL?
robots.txt file me:

**https://insecure-website.com/robots.txt**

Ye file batati hai search engines ko kya crawl nahi karna.
Lekin kabhi kabhi attackers is file se hidden admin pages dhoond lete hain

Page source / JavaScript files se

Guessing ya brute force se
(e.g., try karo: /admin, /dashboard, /config, /panel)

### 🛠️ Real-World Pentesting Tip:
Situation	Tumhara Action

- User ho aur admin page ka guess karo	Check karo kya access milta hai?
- robots.txt mile	Dekho koi sensitive URLs to nahi diye?
Dashboard me koi button missing ho	URL manually type karo aur dekh lo open hota hai ya nahi

### 🧠 Summary in Your Style:
- Broken Access Control:	Chor ko police station me ghusne ki ijaazat mil gayi
- Vertical Escalation:	Student ko principal ke office ka control mil gaya
- Unprotected Function:	Door band nahi kiya gaya, koi bhi andar aa gaya

### 🛡️ Penetration Testing Tip:
Jab bhi access control test karo to:

URL guessing karo (admin panel dhoondo)

Check karo ke normal user admin feature access kar sakta hai ya nahi

BurpSuite ka repeater use karo parameters change karke unauthorized actions try karne ke liye
