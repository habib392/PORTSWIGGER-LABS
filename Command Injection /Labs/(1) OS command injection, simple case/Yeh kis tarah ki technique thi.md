### Iss lab main kya khaas baat thi?

* Server user input ko directly system command main dal raha tha.
* Jo command humne di (jaise `|whoami`) uska output seedha response main show ho gaya.
* Matlab yeh **non-blind injection** tha → hum direct apni command ka result dekh sakte the.

---

### Iss lab ke main points kya thy?

1. Input parameter: `storeId` vulnerable tha.
2. Humne `|whoami` inject kiya aur current user ka naam mil gaya.
3. Is type ke lab main attacker ko turant system access ka pata chal jata hai.
4. Pipe operator (`|`) use hua jo ek command ko dusre command ke sath run karne ke liye hota hai.

---

### Developer ko kya ghalti nahi karni chahiye thi?

1. **Direct shell commands use nahi karni chahiye thi.** Safe libraries ya APIs ka use karna chahiye tha.
2. **Blacklist par bharosa nahi karna chahiye tha.** Agar blacklist hoti to attacker bypass kar sakta tha.
3. **Whitelist input validation** karni chahiye thi (jaise `storeId` sirf numbers ho, aur kuch nahi).
4. **Least privilege principle** use karna chahiye tha – web app ko root ya powerful user ke naam pe run nahi karna chahiye.

---

### Iss lab main kon kon sa point weak ya vulnerable tha?

* User input bina filter ke directly command main lag gaya (input validation nahi thi).
* Output user ko directly response main mil gaya (isse attacker easily test kar saka).
* Application ka design flawed tha, kyunki unnecessary system commands use kar raha tha.

---

### Kya iss tarah ki vulnerability aaj bhi milti hai?

Haan, milti hai – lekin kam.

* Purani applications main yeh zyada hoti hai.
* Agar developer ne **input validation**, **safe APIs**, aur **secure coding practices** ignore kiye hon to abhi bhi mil sakti hai.
* Mostly yeh developer ki wajah se hoti hai, kyunki unhone shortcuts liye hote hain (direct shell call kar di instead of safe code).

---

### Final Summary

OS Command Injection ek dangerous vulnerability hai jisme attacker apne input ko system command ke sath jod deta hai. Agar output user ko directly mil jaye to exploitation aur easy ho jaata hai. Is lab ne yeh sikhaya ke developer ki careless coding (direct shell calls, validation ka na hona) sabse badi wajah hai aisi vulnerabilities ki. Real world main yeh kam milti hai, lekin agar mil jaaye to attacker ko full server access tak le ja sakti hai.

---

💡 **Penetration Tester ka note:**

* Hamesha different operators (`;`, `|`, `&`, `&&`, `||`) test karo.
* Agar output aa raha hai → non-blind injection hai.
* Agar output nahi aa raha → blind injection ho sakti hai.

