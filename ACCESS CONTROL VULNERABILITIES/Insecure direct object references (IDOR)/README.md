# 🔓 IDOR ka matlab kya hota hai?
IDOR (Insecure Direct Object Reference) aik aisi vulnerability hai jo Access Control flaws ka hissa hoti hai.

## 💥 IDOR kab hoti hai?
Jab koi website ya app:

Kisi cheez (jaise user ka profile, order, file waghera) ka access user ke diye gaye input se karti hai.

Aur wo input agar attacker manually change kar de, aur phir bina permission ke doosre user ka data mil jaye, to usay hi IDOR kehte hain.

### 🎯 Example (Tere style mein):
Tu logged in hai wiener ke naam se aur URL kuch aisa hai:

**https://site.com/account?id=wiener**

Ab tu **id=wiener** ki jagah **id=carlos** likhta hai, aur Carlos ka account khul jata hai —
Toh yehi IDOR vulnerability hai bhai!

### 📜 History:
IDOR itni common vulnerability thi ke OWASP Top 10 – 2007 mein bhi uska zikr aya tha.

Iska matlab yeh ke ye purani aur dangerous vulnerability hai, jo abhi tak kai websites mein milti hai.

### ⚠️ Simple Words:
“IDOR ka matlab hota hai ke koi app user ke diye gaye input se kisi cheez tak direct pohanch jaati hai, aur agar attacker wo input change karke unauthorized cheez tak pohanch jaye, to yeh access control ka bug ban jaata hai.”

### 💡 Pentester Tip:
Har jagah jahan URL, form, ya request mein ID / username / file name / order number dikhe — usay change karke test karo, kahin IDOR chhupa na ho!
