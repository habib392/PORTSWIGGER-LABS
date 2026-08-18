### Ek Hi Column Mein Multiple Values Nikalna
Baaz auqaat original query se sirf **ek hi column** return ho raha hota hai, lekin aapko username aur password dono ek saath nikalne hotay hain.
Is maslay ko hal karne ke liye aap dono values ko **Concatenate** (aapas mein jodh) dete hain aur beech mein ek separator (jaise ~ ya :) laga dete hain taake values alag alag pehchani ja sakein.
**Misaal (Oracle Database Par):**
```sql
' UNION SELECT username || '~' || password FROM users--

```
 * Oracle mein double-pipe (||) string ko jodne (concatenation) ke liye istemal hota hai.
 * Yeh query username aur password ko jodh kar beech mein ~ ka nishan daal degi.
**Result Kaise Dikhayega?**
Screen par aapko data is tarah juda hua milega:
 * administrator~s3cure
 * wiener~peter
 * carlos~montoya
**Doosre Databases Mein Fark:**
Concatenation ka syntax har database mein alag hota hai:
 * **MySQL / PostgreSQL:** CONCAT(username, '~', password) ya username || '~' || password
 * **MSSQL:** username + '~' + password

---

## IMPORTANT NOTE

command main space lagany sy zyada farq nhi parhta command phir bhi execute hoo hi jati hai jaisy yeh command hai

UNION SELECT username || '~' || password FROM users--

iss main agar yeh changing hoo jaye 

UNION SELECT username||'~'||password FROM users--

phir bhi yeh execute hoo jaye hi, or asal command yeh hai

UNION SELECT NULL,username || '~' || password FROM users--

Yani 2 columns hoty hain isko pehly identify krna hota hai ky kis column main text data jaa rha hai agar 2nd column main text data jaa rha hai too wahan NULL,username || '~' || password FROM users-- likhein gy. yahan NULL first column hai or dosray column main username or password ko combine kiya gya hai string concatenation ki wajah sy phir yeh  execute hua