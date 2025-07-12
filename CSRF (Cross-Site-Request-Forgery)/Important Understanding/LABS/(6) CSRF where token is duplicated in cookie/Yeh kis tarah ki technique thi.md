CSRF token cookie aur request dono mein hona — "Double Submit" technique

Kuch websites CSRF token ko server side pe store nahi karti. Balkay wo har request mein token ko 2 jagah check karti hain:

1. Request body mein (jaise form ke andar)

2. Cookie mein (jo browser ke saath chali aati hai)

Phir server bas itna check karta hai:

> "Kya dono jagah ka token same hai?"

Agar haan, toh wo request ko valid maan leta hai.
Isay “Double Submit Cookie Defense” kehte hain.


---

❗ Ismein attacker kya kar sakta hai?

Attacker:

Apna khud ka token generate karta hai (chahe wo fake hi kyun na ho)

Apni site (ya kisi subdomain) ke zariye victim ke browser mein ek cookie set karta hai jismein wo token hota hai

Phir CSRF attack ke zariye victim se wahi token waali request bhejta hai


Server yeh sochta hai ke “yeh token valid hai, dono jagah same hai”, lekin asal mein attacker ne hi dono jagah ka token set kiya hota hai.


---

🛠️ Real-World PenTesting Tip:

Jab bhi double submit token mechanism dekho, toh check karo:

Kya attacker cookie set kar sakta hai? (subdomain se ya koi open endpoint se)

Kya token validate ho bhi raha hai ya sirf match check ho raha hai?
