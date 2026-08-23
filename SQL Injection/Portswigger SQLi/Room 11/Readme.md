### Verbose Error Messages Ke Zariye Sensitive Data Nikalna
Aksar database ki **Misconfiguration** (Ghalat Setting) ki wajah se application bohot zyada detailed (verbose) error messages screen par dikha deti hai. Yeh error messages attacker ke liye bohot faide-mand hote hain.
**Misaal:**
Maan lein jab hum id parameter mein ek single quote (') daalte hain, toh screen par yeh error aata hai:
> *Unterminated string literal started at position 52 in SQL SELECT * FROM tracking WHERE id = '''. Expected char*
> 
### Is Error Se Kya Faida Milta Hai?
 1. **Poori Query Nazar Aa Jati Hai:** Error message se humein backend ki poori query (SELECT * FROM tracking WHERE id = ...) ka pata chal jata hai.
 2. **Injection Point Ka Pata Chalta Hai:** Humein saaf dikh jata hai ke hamara input ek single-quoted string ('...') ke andar WHERE clause mein ja raha hai.
 3. **Payload Banana Aasan Ho Jata Hai:** Jab backend query ka structure pehle se pata ho, toh ek sahi payload banana bohot aasan ho jata hai. Hum baki bachi hui query ko comment out (-- ya #) karke syntax error se bach sakte hain aur apni query execute karwa sakte hain.

---

### CAST() Function Ke Zariye Error Mein Directly Data Print Karwana
Kabhi kabhi aap application se aisa error trigger karwa sakte hain jiske andar database ka **actual sensitive data hi print ho kar bahar aa jata hai**. Is se jo SQL injection pehle blind (chupa hua) tha, woh mukammal taur par visible (nazar aane wala) ban jata hai.
**Mechanism (CAST() Function Ka Istemal):**
CAST() function ek data type ko doosri type mein badalney ke liye istemal hota hai.
**Misaal:**
Maan lein aap yeh query inject karte hain:
```sql
CAST((SELECT example_column FROM example_table) AS int)

```
**Hasil-e-Kalam (Result):**
Aap jis data (maslan password ya username) ko read karne ki koshish kar rahe hain woh text/string hota hai. Jab database us text string ko Integer (int yani number) mein convert karne ki koshish karega, toh conversion fail ho jayegi aur database screen par yeh error de dega:
> ERROR: invalid input syntax for type integer: "Example data"
> 
**Faida:**
Error message ke andar hi "Example data" ki jagah aapka chahie hua **Password ya Data print ho kar samne aa jayega**! Aapko ek ek character karke brute force karne ki zaroorat hi nahi paregi. Yeh tareeqa wahan bhi bohot kaam aata hai jahan character limit ki wajah se lambi conditional queries na chal sakti hon.

---

Database conversion fail hone par error dega aur usi error message ke andar hi target data (jaise password) print ho kar bahar aa jayega.

Databases mein **Data Types** ka matlab hota hai ke computer ko batana ke kisi column ya value mein kis kism ka data save ho raha hai.

### Data Types Ka Faraq
 * **Integer (int):**
   * int ka matlab hota hai **Whole Numbers** (poore adad/ginti wale numbers) bina kisi point/decimal ke.
   * **Examples:** 1, 45, 100, 9999, -50
   * Complex math aur ginti ke liye int use hota hai. Isme alphabetical characters (a-z) ya symbols allowed nahi hotay.
 * **String (varchar / text):**
   * Jo data single ya double quotes ke andar hota hai. Isme letters, numbers, aur symbols sab aa sakte hain.
   * **Examples:** 'admin', 'password123', 'hsjambjanbwx'
### Error Kyun Trigger Hota Hai?
Jab tum database ko kehte ho ke 'password123' (string) ko **int (number)** mein convert kare:
 1. Database dekhta hai ke 'password123' ke andar p, a, s, s jaise letters maujood hain.
 2. Math logic ke hisaab se letters ko number (int) nahi banaya ja sakta.
 3. Database crash hokar error generate karta hai:
   > ERROR: invalid input syntax for integer: "password123"
   > 
Is tarah database process fail karne ki wajah se woh text/string value (jo hum extract karna chahte the) error message ke andar hi expose kar deta hai.

---

Yeh Data Types (**int**, **varchar**, **text**, waghaira) **SQL (Database Query Language)** ki apni types hain.
Lekin yeh concept taqreeban har programming language mein hota hai. Aayein dekhte hain ke yeh alag alag jagahon par kaise use hota hai:
### Data Types Ka Faraq & Usage
 * **SQL (Database Language):**
   * Jab hum database mein tables banate hain (jaise MySQL, PostgreSQL, Oracle), toh hum SQL mein specify karte hain ke kis column mein kya data aayega (e.g., age INT, username VARCHAR).
   * Hum ne jo CAST(... AS int) use kiya tha, woh **SQL language** ka command tha.
 * **Backend Languages (Python, PHP, Node.js, Java):**
   * Backend code mein bhi data types hoti hain (jaise Python mein int, str, float).
   * Backend language SQL query ko execute karne ke liye database ko bhejti hai.
 * **Frontend Language (JavaScript):**
   * JavaScript mein bhi numbers aur strings ki types hoti hain (e.g., Number, String).

### Summary
Data Types har language ka bunyadi hissedar (building block) hoti hain. SQL injection ke dauran jo error hum paida karte hain, woh **SQL Database System** ka apna error hota hai kyunki hum SQL command ke andar CAST() function se Database ko force kar rahe hotay hain.


