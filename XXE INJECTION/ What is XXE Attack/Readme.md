### XML Kya Hai?
XML ka matlab hai **"Extensible Markup Language"**. Yeh ek aisi zuban hai jo data ko store aur transfer karne ke liye use hoti hai. Soch, yeh ek tarah ka box hai jisme tu apna data rakh sakta hai. HTML ki tarah isme bhi tags hote hain, lekin yahan tags predefined nahi hote. Tu khud apne data ke hisaab se tags bana sakta hai. Pehle ke zamanay mein XML bohot popular tha data bhejne ke liye (jaise AJAX mein "X" ka matlab XML hai), lekin ab JSON zyada use hota hai.

### XML Entities Kya Hain?
XML entities ek tarika hain jisme data ko seedha likhne ke bajaye uski jagah kuch aur likha jata hai. Jaise agar tujhe "<" ya ">" likhna ho, toh yeh XML tags ke liye use hote hain, isliye inko direct nahi likh sakte. Inke bajaye **&lt;** aur **&gt;** likha jata hai. Yeh entities XML ke rulebook ka hissa hain.

**Example**: Agar tu "<" likhega to XML confuse ho jayega, isliye tu **&lt;** likhta hai, jo batata hai ke yeh "<" sign hai.

### Document Type Definition (DTD) Kya Hai?
DTD ek tarah ka rulebook hai jo batata hai ke XML document kaisa hona chahiye, usme kaunsa data ho sakta hai, aur uski structure kya hogi. Yeh DTD document ke shuru mein **<!DOCTYPE>** ke andar likha jata hai. Yeh do tarah ka ho sakta hai:
- **Internal DTD**: Jo document ke andar hi likha hota hai.
- **External DTD**: Jo kahin aur se load hota hai.
- Ya dono ka mix bhi ho sakta hai.

### XML Custom Entities Kya Hain?
Tu apne khud ke entities bana sakta hai DTD ke andar. Matlab, tu ek chhota sa code likh ke bol sakta hai ke jab bhi yeh entity dikhe, toh uski jagah yeh specific value daal do.

**Example**:
```xml
<!DOCTYPE foo [ <!ENTITY myentity "mera value" > ]>
```
Ab jab bhi XML document mein **&myentity;** likha jayega, toh yeh "mera value" se replace ho jayega.

### XML External Entities Kya Hain?
External entities bhi custom entities hain, lekin inki value kahin aur se aati hai, jaise kisi website ya file se. Inka definition **SYSTEM** keyword ke saath hota hai aur ek URL diya jata hai.

**Example**:
```xml
<!DOCTYPE foo [ <!ENTITY ext SYSTEM "http://example.com" > ]>
```
Yeh batata hai ke **&ext;** ki jagah woh data ayega jo http://example.com se load hoga.

Aur agar file se data lena ho, toh aisa:
```xml
<!DOCTYPE foo [ <!ENTITY ext SYSTEM "file:///path/to/file" > ]>
```
Yeh external entities hi **XXE (XML External Entity) attacks** ka main zariya hain, kyunki hackers iska galat istemal kar sakte hain.

---

