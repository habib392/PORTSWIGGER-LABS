# HTTP Basic Authentication
HTTP Basic Authentication kya hoti hai?
Yeh purani aur simple tareeqe ki authentication hoti hai jo aaj bhi kabhi kabhi websites mein mil jaati hai — zyaada asaan aur jaldi implement hone wali hoti hai.

### Kaise kaam karti hai?
Jab user login karta hai, to username aur password ko jod kar ek string banti hai:

username:password

Yeh string Base64 mein convert hoti hai (jaise encryption jaisa hota hai lekin secure nahi hota).

Har baar jab browser koi request bhejta hai, to yeh token Authorization header mein bhej deta hai:

Authorization: Basic base64(username:password)

Yeh secure kyun nahi hai?

Har request ke sath username aur password baar baar bheja jaata hai — agar website HTTPS ya HSTS ka use nahi karti, to attacker beech mein data chura sakta hai (man-in-the-middle attack).

Brute-force protection nahi hoti — attacker bar bar Base64 token try karta hai jab tak sahi na mil jaaye.

Session control nahi hota — jaise CSRF attacks ko rok nahi sakta, user session ka control weak hota hai.

### Aur nuksan kya hai?
Kabhi kabhi attacker sirf ek simple page tak pohchta hai, lekin us page ke credentials dusri jagah bhi use ho sakte hain (jaise admin panel ya private data), to attacker aur aage barh sakta hai.

Yeh old method hai, aaj kal secure websites isay use nahi karti.
