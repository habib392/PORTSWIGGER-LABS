### **Insecure Deserialization Kya Hai?**

**Insecure deserialization** tab hoti hai jab koi website user ke control mein hone wala data (jo serialized form mein hota hai) ko deserialize karti hai. Isse attacker website ke code mein khatarnak data daal sakta hai. Yeh ek serious security issue hai kyunki isse website ko bohot bade attacks ka samna karna par sakta hai, jaise remote code execution, data chori, ya denial-of-service attacks.

**Example:** Socho ke tum ek website pe ek form fill karte ho, aur us form ka data server pe serialized form mein jata hai. Agar server us data ko blindly deserialize kare aur usme attacker ne kuch malicious code daal diya ho, to yeh code website ke system mein execute ho sakta hai.

---

### **Serialization Kya Hai?**

**Serialization** ek process hai jisme complex data structures (jaise objects aur unke fields) ko ek simple, flat format mein convert kiya jata hai, jo ek sequential stream of bytes hota hai. Iska faida yeh hai ke:

1. **Complex data save karna asaan ho jata hai**: Jaise memory, file, ya database mein.
2. **Data transfer karna simple hota hai**: Network ke through, ya app ke different parts ke beech, ya API calls mein.

Jab object serialize hota hai, to uska **state** bhi save hota hai, yani uske attributes aur unki values bhi preserve hoti hain.

**Example:** Ek user object hai jisme name, age, aur email hai. Jab yeh serialize hota hai, to yeh ek string ya binary format mein convert ho jata hai, jaise:  
`{"name":"Ali","age":25,"email":"ali@example.com"}`

---

### **Deserialization Kya Hai?**

**Deserialization** serialization ka ulta process hai. Yani, jo byte stream ya flat data serialize kiya gaya tha, usse wapas original object mein convert karna. Yeh object bilkul waise hi hota hai jaise woh serialize hone se pehle tha, aur website ka code isse normally use kar sakta hai.

**Example:** Upar wala serialized data jab deserialize hoga, to wapas ek user object banega jisme name, age, aur email wahi honge jo pehle the.

**Serialization vs Deserialization:**  
- **Serialization**: Object ko byte stream mein convert karna.  
- **Deserialization**: Byte stream ko wapas object mein convert karna.

---

### **Insecure Deserialization Ka Masla Kya Hai?**

Jab website user ke data ko deserialize karti hai, aur woh data attacker ne manipulate kiya ho, to yeh **insecure deserialization** kehlati hai. Isse attacker:

1. **Malicious objects inject kar sakta hai**: Woh serialized data mein aisa object daal sakta hai jo website ke code mein khatarnak kaam kare.
2. **Unexpected class ka object ban sakta hai**: Yani, website ko lagta hai ke ek specific type ka object aayega, lekin attacker kisi aur class ka object bhej deta hai. Yeh "object injection" kehlata hai.
3. **Attack deserialization ke dauraan hi shuru ho sakta hai**: Matlab, website ka code us object ke saath interact karne se pehle hi attack ho sakta hai.

**Example:** Ek website expect karti hai ke ek "User" object deserialize hoga. Lekin attacker ek "Malicious" object bhej deta hai jo server pe khatarnak code chala deta hai, jaise system files delete karna.

---

### **Yeh Vulnerability Kyun Hoti Hai?**

Insecure deserialization ke masle aksar is wajah se hote hain:

1. **Developers ko samajh nahi hoti**: Logon ko lagta hai ke user data deserialize karna safe hai, lekin yeh bohot risky hota hai.
2. **Galat checks lagaye jate hain**: Developers sochte hain ke deserialized data ko check karna kaafi hai, lekin yeh checks aksar fail hote hain kyunki attack deserialization ke waqt hi ho jata hai.
3. **Binary formats ko safe samajhna**: Binary serialized data ko manipulate karna mushkil lagta hai, lekin attackers isse bhi hack kar sakte hain.
4. **Dependencies ka jhanjhat**: Modern websites mein bohot si libraries hoti hain, aur unke apne dependencies hote hain. Inme se koi bhi class attacker ke liye entry point ban sakti hai.

**Example:** Ek website PHP mein ek library use karti hai jo user data deserialize karti hai. Attacker us library ke ek known bug ka faida utha kar malicious code inject kar deta hai.

---

### **Insecure Deserialization Ka Impact Kya Hai?**

Yeh vulnerability bohot khatarnak hai kyunki:

1. **Remote Code Execution (RCE)**: Attacker website ke server pe apna code chala sakta hai.
2. **Privilege Escalation**: Attacker apne user permissions ko badha sakta hai, jaise admin ban jana.
3. **Arbitrary File Access**: Server ke sensitive files tak pohonch sakta hai.
4. **Denial-of-Service (DoS)**: Website ko crash kar sakta hai.

**Example:** Ek attacker ek malicious object deserialize karwata hai jo server ke database ko delete kar deta hai. Yeh website ke liye bara nuksaan hai.

---

### **Insecure Deserialization Ko Kaise Exploit Kiya Jata Hai?**

Attackers is vulnerability ko exploit karne ke liye yeh steps follow karte hain:

1. **Serialized data manipulate karna**: Woh serialized data mein malicious code ya object daal dete hain.
2. **Gadget chains use karna**: Yeh ek series hoti hai jisme attacker ek object ke methods ko chain ki tarah use karta hai taake khatarnak kaam ho sake.
3. **Unexpected classes ka faida uthana**: Attacker aisi class ka object bhejta hai jo website expect nahi karti, lekin jo server pe available hoti hai.

**Example (PHP):** Ek PHP website user input deserialize karti hai. Attacker ek serialized object bhejta hai jo PHP ke `__destruct` method ko trigger karta hai aur server pe malicious command chala deta hai.

**Example (Ruby):** Ruby mein `YAML.load` function use hota hai. Attacker ek malicious YAML string bhejta hai jo server pe arbitrary code execute karta hai.

**Example (Java):** Java mein `ObjectInputStream` ke through attacker ek object bhej sakta hai jo server ke sensitive methods ko call kare.

---

### **Insecure Deserialization Ko Kaise Rokain?**

Is vulnerability se bachne ke liye yeh steps follow karo:

1. **User input deserialize na karo**: Agar bilkul zaroori na ho, to user data ko deserialize karne se bacho.
2. **Data integrity check karo**: Deserialization se pehle data ka digital signature ya checksum verify karo taake yeh confirm ho ke data tamper nahi hua.
3. **Custom serialization use karo**: Generic serialization ke bajaye apni class-specific serialization banayein, jisme sirf zaroori fields expose hon.
4. **Gadget chains pe focus na karo**: Yeh sochna ke saari gadget chains ko fix kar denge, impractical hai kyunki dependencies bohot hoti hain.

**Example:** Ek website user data ko deserialize karne se pehle uska HMAC signature check karti hai. Agar signature match nahi karta, to data reject ho jata hai.

---

### **Programming Languages Mein Serialization**

- **PHP**: Objects ko `serialize()` function se string mein convert kiya jata hai, aur `unserialize()` se wapas object banaya jata hai.
- **Ruby**: Ruby mein serialization ko "marshalling" kehte hain, aur `Marshal.dump` aur `Marshal.load` use hote hain.
- **Python**: Python mein serialization ko "pickling" kehte hain, aur `pickle` module use hota hai.
- **Java**: Java mein `ObjectOutputStream` aur `ObjectInputStream` use hote hain.

**Note:** Har language ka format alag hota hai. Kuch languages binary format use karte hain (jaise Java), aur kuch string-based (jaise PHP). Lekin attacker dono ko manipulate kar sakta hai.

---

### **Real-World Example**

Socho ek online shopping website hai jo user ke cart ko serialized form mein save karti hai. Ek attacker cart data mein ek malicious object inject karta hai jo checkout ke waqt server pe khatarnak code chala deta hai, jaise admin access hasil karna. Yeh insecure deserialization ka practical example hai.

---

### Example of Insecure Deserialization 
Jab aik banda jaisy ali kisi dosray bandy ahmed ko aik box bhejta hai jis main kuch gifts hoty hain lekin agr beech main woh daba main check karon or ius main sy gifts ko nikal kr fazool cheezein jaisy pathar daal kr box ko band kr ky aagy bhej wadoon too jab yeh parcel ahmed ky pass pohanchy ga or ahmed iss parcel ko kholy ga too ius ko gifts ki jagah pathar milein gyein isi ko kehty hain Insecure Deserialization.


