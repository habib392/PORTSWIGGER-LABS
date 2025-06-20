# 🔍 Yeh kis tarah ki technique thi?

### ✅ Insecure Direct Object Reference (IDOR) in Static Files
Application ne user ka chat data server ke file system mein save kiya.

Har chat ka ek .txt file ban gaya — jaise: 6.txt, 7.txt, 8.txt

File ka naam incrementing number pe based tha — predictable!

Koi authentication ya access control nahi tha.

Attacker ne sirf number change karke doosre user ka sensitive data nikaal liya (Carlos ka password).

### ⚔️ Penetration Tester isko kaise use karta hai?
🎯 Real-world mein jab pentester ko lage:
Static resources serve ho rahe hain (/static/file.txt, /downloads/101.pdf, etc.)

File ka naam simple number, ID ya date/time pattern pe ho.

Koi login ya permission check nahi hai file access pe.

### 🕵️‍♂️ Tab pentester yeh steps karta hai:
Kisi apni file ka URL note karta hai

Usi format mein number ya filename change karta hai

BurpSuite se test karta hai ke kya doosri file mil rahi hai

Agar sensitive info mil jaye (credentials, chats, reports, invoices)
→ to wo IDOR ke under report karta hai
→ severity: High or Critical

### 🧠 Summary in 1 line:
“Yeh technique attacker ko doosre user ki files tak pohanch deti hai — sirf file ka naam guess ya change karke — bina kisi authentication ke.”

### 💡 Real bug bounty tip:
Jab bhi tu kisi site pe .txt, .pdf, .json, .jpg file URLs dekhe jo predictable lag rahe hon, to turant file number/ID change karke test kar — IDOR ka chance hota hai!
