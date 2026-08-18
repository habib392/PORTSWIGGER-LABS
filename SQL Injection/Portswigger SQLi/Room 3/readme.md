### SQL Injection UNION Attacks
Jab kisi application mein SQL injection ki vulnerability ho aur query ka result screen par nazar aa raha ho, toh aap database ki doosri tables se data nikalne ke liye UNION keyword ka istemal kar sakte hain. Isko **SQL injection UNION attack** kehte hain.
UNION operator ka kaam yeh hota hai ke yeh original query ke saath ek ya zyada nayi SELECT queries ko jodh (append kar) deta hai.
**Misaal:**
```sql
SELECT a, b FROM table1 UNION SELECT c, d FROM table2

```
Yeh query dono tables se data jama karke ek hi result set mein dikhayegi, jisme do columns honge — ek taraf table1 ke a, b aur doosri taraf table2 ke c, d.

---

### SQL Injection UNION Attacks (Taqaza / Requirements)
UNION query ko sahi tareeqay se chalane ke liye do (2) zaroori shartain poori hona lazmi hain:
 1. **Columns ki tadaad barabar honi chahiye:** Dono queries se aane wale columns ki tadad bilkul aik jitni honi chahiye.
 2. **Data types match honi chahiye:** Dono queries ke aamne-saamne wale columns ki data types (jaise Text, Numbers wagairah) aapas mein milti julti / compatible honi chahiye.
Kamyab UNION attack karne ke liye aap ko pehle yeh do cheezein find out karni parti hain:
 * Original query se kul kitne columns return ho rahe hain.
 * Un columns mein se konse columns aisi data type ke hain jin mein aap apna injected data (jaise text ya string) fit kar sakte hain.

---

### Columns Ki Tadaad Maaloom Karna
UNION attack karne ke liye original query se kitne columns aa rahe hain, yeh pata lagane ke 2 behtareen tareeqay hain. Pehla tareeqa ORDER BY ka hai.
Is method mein aap ek ke baad ek ORDER BY ki ginti (index) barha kar bhejte hain jab tak database error na de de.
**Payloads Ki Misaal:**
```text
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--

```
**Yeh Kaise Kaam Karta Hai?**
ORDER BY mein column ka naam likhne ki zaroorat nahi hoti, sirf uska number (index) dena kafi hota hai.
 * Jab aap ORDER BY 1-- bhejte hain, toh database 1st column se data sort karta hai.
 * Jab aap ginti barhate hue aisi raqam par pahunchte hain jo total columns se zyada ho (maslan total 2 columns hain aur aapne ORDER BY 3-- bheja), toh database error de deta hai.
**Response Kaise Pehchanein?**
 * **Direct Error:** Application screen par database ka error dikha sakti hai (jaise: *"ORDER BY position number 3 is out of range"*).
 * **Generic Error:** Ya phir koi aam sa error / 500 status code aa sakta hai.
 * **Blank Page:** Ya phir page bilkul khali (no results) ho sakta hai.
Jaise hi aapko response mein koi tabdeeli nazar aaye, aapko pata chal jata hai ke purani ginti tak hi total columns the.

---

### Columns Ki Tadaad Maaloom Karna (Dusra Tareeqa: NULL Values)
Dusra tareeqa yeh hai ke aap UNION SELECT ke sath barhte hue NULL values bhejte hain jab tak sahi tadaad na mil jaye.
**Payloads Ki Misaal:**
```text
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--

```
**Hum NULL Kyun Istemal Karte Hain?**
Aapko pata hai ke UNION query ke liye data types ka match hona zaroori hai. NULL har aam data type (text, number, date) mein convert ho jata hai, is liye jab columns ki ginti sahi ho jati hai toh error aane ka chance khatam ho jata hai.
**Result Aur Response Kaise Pehchanein?**
 1. **Galat Columns Count:** Agar NULL ki tadaad original columns se match nahi karegi, toh database error dega (maslan: *"All queries combined must have equal expressions"*).
 2. **Sahi Columns Count:** Jab NULL ki ginti bilkul barabar ho jayegi, toh database bina kisi SQL error ke query run kar dega aur NULL values wali ek extra row return karega.
**Response Mein Kya Fark Dikh Sakta Hai?**
 * **Extra Data:** HTML table mein koi khali ya extra row/space nazar aa sakti hai.
 * **Application Error:** Kuch applications NULL data handle nahi kar paatein, toh code par NullPointerException jaisa error aa sakta hai (jo ke SQL error se alag hota hai).
 * **Kuch Naya Na Dikhna:** Agar response mein koi farq nazar na aaye, toh yeh method wahan nakaam ho jata hai aur hum pehla (ORDER BY) method hi prefer karte hain.


