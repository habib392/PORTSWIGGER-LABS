### **SQL Injection (SQLi) Kya Hai?**

SQL injection (SQLi) aik web security vulnerability (kamzori) hai jo attacker ko ijazat deti hai ky woh application ki taraf sy database ko bheji jani wali queries main rukawat daliye ya apni marzi ki commands chalaye. Iss ki madad sy attacker woh data dekh sakta hai jo aam taur par usy nazar nahi aata—jaise baaqi users ka personal data ya database ka doosra sensitive content. Unme sy aksar cases main, attacker iss data ko badal (modify) bhi sakta hai ya permanently delete bhi kar sakta hai, jis sy application ka behavior badal jata hai.

Kuch scenarios main, attacker SQL injection attack ko aglay level par le ja kar poore underlying server ya back-end infrastructure ko compromise kar sakta hai, aur yeh Denial-of-Service (DoS) attacks karne ke liye bhi istemal ho sakta hai.

---

### **SQL Injection Vulnerabilities Ko Kaise Detect (Pehchana) Jata Hai?**

Aap SQL injection ko manually detect karne ke liye application ke har entry point par aik tareeqe se tests ka poora set chala sakte hain. Iss ke liye aam taur par yeh chizen submit ki jati hain:

* **Single Quote (`'`):** Entry point par single quote daal kar yeh dekha jata hai ke kya application koi error ya weird behavior (anomaly) dikhati hai.
* **SQL-Specific Syntax:** Aisa SQL syntax bheja jata hai jo calculate hone par entry point ki original (base) value deta hai, aur phir aisa jo doosri value deta hai—phir dono suraton mein application ke responses ka farq dekha jata hai.
* **Boolean Conditions:** `OR 1=1` aur `OR 1=2` jaise logic condition payloads bhej kar application ke response mein aane wale badlao ko check kiya jata hai.
* **Time Delays (Time-Based):** Aise payloads bheje jate hain jo SQL query ke andar execute hone par response mein waqt ka waqfa (delay) paida karte hain, aur phir response aane ke time ka farq dekha jata hai.
* **OAST Payloads (Out-of-Band):** Aise payloads jo SQL query mein chalne par out-of-band network interaction (external server par request trigger) karte hain, aur phir resulting interactions ko monitor kiya jata hai.

Iss ke ilawa, aap Burp Scanner ka istemal kar ke aksar SQL injection vulnerabilities ko jaldi aur bina kisi ghalti ke dhoond sakte hain.

---

### **Query Ke Alag Alag Hisson Main SQL Injection**

Aksar SQL injection vulnerabilities `SELECT` query ke `WHERE` clause ke andar milti hain, aur zyadatar experienced testers iss kisam ke SQLi se achi tarah waqif hotay hain.

Magar, SQL injection query ke kisi bhi hissay main aur alag alag query types ke andar ho sakti hai. Kuch aam jaghein jahan SQLi paida hoti hai:

* **UPDATE Statements:** Updated values ke andar ya `WHERE` clause ke andar.
* **INSERT Statements:** Insert ki jane wali values ke andar.
* **SELECT Statements (Table/Column):** Table ke naam ya column ke naam ke andar.
* **SELECT Statements (ORDER BY):** `ORDER BY` clause ke andar.

---

### **Hidden Data Ko Retrieve (Zahir) Karna**

Farz karein aik shopping application hai jo alag alag categories ke products dikhati hai. Jab user **Gifts** category par click karta hai, toh uska browser yeh URL request bhejta hai:

`[https://insecure-website.com/products?category=Gifts](https://insecure-website.com/products?category=Gifts)`

Iss waja se application database se relevant products ki detail nikalne ke liye yeh SQL query chalati hai:

`SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

Yeh SQL query database se maangti hai:

* Tamaam details (`*`)
* `products` table se
* Jahan category `Gifts` ho
* Aur `released` ki value `1` ho.

Yeh `released = 1` ki shart (restriction) un products ko chupane ke liye istemal ho rahi hai jo abhi release nahi huay. Hum maan sakte hain ke unreleased products ke liye `released = 0` hoga.

Application ke andar SQL injection attacks se bachne ke liye koi defense/security nahi hai. Iss ka matlab hai ke attacker, misaal ke taur par, yeh attack tayyar kar sakta hai:

`[https://insecure-website.com/products?category=Gifts'--](https://insecure-website.com/products?category=Gifts'--)`

Iska nateeja yeh SQL query banta hai:

`SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1`

Ghor karne wali baat yeh hai ke SQL main `--` aik **comment indicator** hota hai. Iska matlab hai ke query ka baaqi hissa comment ke taur par samjha jaye ga, yani woh khatam (remove) ho jaye ga. Iss misaal main, query se `AND released = 1` nikal jata hai. Nateeje ke taur par, tamaam products dikhai dene lagte hain, un-released products samait.

Aap isi tarah ka attack kar ke application se kisi bhi category ke saare products show karwa sakte hain, un categories ke bhi jo aam logon ko pata nahi hain:

`[https://insecure-website.com/products?category=Gifts'+OR+1=1--](https://insecure-website.com/products?category=Gifts'+OR+1=1--)`

Iska nateeja yeh SQL query banta hai:

`SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1`

Yeh badli hui query un tamaam items ko wapas laati hai jahan ya toh category `Gifts` ho, ya phir `1=1` ho. Kyunki `1=1` hamesha **TRUE** hota hai, iss liye query tamaam items return kar deti hai.

**Warning (Inthabah)**

SQL query ke andar `OR 1=1` condition inject karte waqt hamesha ehtiyaat karein. Agarcha yeh uss jagah be-zarar (harmless) lag sakti hai jahan aap inject kar rahe hain, magar applications aksar aik hi request ke data ko doosri alag alag queries main bhi istemal karti hain. Agar aap ki yeh condition kisi `UPDATE` ya `DELETE` statement tak pahunch gayi, toh isse ghalti se poore database ka data delete ya kharab (loss) ho sakta hai.
