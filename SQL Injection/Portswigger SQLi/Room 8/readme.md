### Database Ke Materials / Contents Ki List Nikalna
Oracle ke ilawa baaqi aksar databases (jaise MySQL, PostgreSQL, MSSQL) mein views ka ek set hota hai jise **information_schema** kehte hain. Yeh poore database ki information aur naqsha fraham karta hai.
**1. Tables Ki List Nikalna (information_schema.tables):**
Database mein kitni aur konsi tables hain, yeh dekhne ke liye yeh query chalayi jati hai:
```sql
SELECT * FROM information_schema.tables

```
**Output Misaal:**
Is query se aisi list milti hai:
 * Products
 * Users
 * Feedback
Is se pata chalta hai ke database mein yeh teen tables maujood hain.
**2. Specific Table Ke Columns Nikalna (information_schema.columns):**
Jab kisi khas table (maslan Users) ke andar ke columns aur unki data type dekhni ho, toh yeh query chalti hai:
```sql
SELECT * FROM information_schema.columns WHERE table_name = 'Users'

```
**Output Misaal:**
Is query se Users table ke ye columns aur data types samne aate hain:
 * UserId (int - number format)
 * Username (varchar - text format)
 * Password (varchar - text format)
Is tarah aapko exact table aur column names ka pata chal jata hai taake aap targeted UNION attack chala sakein.
