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
