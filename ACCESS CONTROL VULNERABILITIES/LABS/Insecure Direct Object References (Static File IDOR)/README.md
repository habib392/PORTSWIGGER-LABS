# 🔓 Lab: Insecure Direct Object References (Static File IDOR)

##🎯 Goal:
Carlos ka password chat transcript se nikalna hai, aur uske account mein login karna hai ✅

### ✅ Step-by-Step Simple Solution:
Live Chat Open Karo:

Lab open karo.

Top menu mein "Live chat" tab pe click karo.

Koi bhi Message Send Karo:

Chat box mein koi bhi message bhejo.

**Example: "hello test"**

View Transcript Option Aayegi:

Message bhejnay ke baad "View transcript" pe click karo.

aik file download hogi jaisy 2.txt isky baad burp main jao HTTP history 
main wahan **GET /download-transcript/5.txt** isky andar ja kr request ko repeater main bhejna hai
isky baad

Filename Change Karo:

7.txt ko change karke 1.txt ya 2.txt try karo.

Jab tak carlos ka password milta hai, files check karte raho.

Carlos ka Password Milne Par:

My account main jao wahan yeh submit karo

Username: carlos

Password: (jo text file mein mila hai)

Login karo ✅

## LAB SOLVED

### ⚔️ Real-World Penetration Testing Use:
Jab koi website user data ko file system pe static tarike se save kare — jaise .txt, .pdf, .json — aur attacker uska file name guess ya manipulate karke doosre users ka data access kar le… to usay IDOR in static files kehte hain.

Yeh bug mostly:

No authentication check

No file access control

Predictable file names (1.txt, 2.txt...) ki wajah se hota hai.

### 📌 Summary in One Line:
“Live chat ka transcript predictable file name se open ho raha hai — attacker sirf file
number change karke carlos ka password nikaal leta hai.”

