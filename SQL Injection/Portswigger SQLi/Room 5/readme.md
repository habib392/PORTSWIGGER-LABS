### Data Nikalney Ke Liye UNION Attack Ka Istemal
Jab aapko total columns ki ginti aur unki data type (string/text) ka pata chal jaye, toh aap database se apna kaam ka data (jaise usernames aur passwords) nikal sakte hain.
**Misaal Ke Tor Par:**
 * Original query se 2 columns aa rahe hain aur dono String support karte hain.
 * Injection point WHERE clause ke andar single quote (') ke saath hai.
 * Database mein users naam ki table hai jisme username aur password ke columns hain.
Aap yeh payload bhej kar poori users table ka data nikal sakte hain:
```sql
' UNION SELECT username, password FROM users--

```
**Zaroori Shart:**
Is attack ko chalane ke liye aapko table ka naam (users) aur columns ke naam (username, password) pata hone chahiye. Agar yeh naam pehle se pata na hon, toh guessing (andaza) karna parta hai ya phir database ki built-in tables (jaise information_schema) se database ka structure check karke asli naam dhoondne parte hain.

---

Jab aapko exact table ya column ka naam na pata ho, toh aap underground tareeqay se database se hi poora naqsha dhoond nikalte hain.

### 1. Information_Schema Kya Bala Hai?
information_schema har modern database (MySQL, PostgreSQL, MSSQL) ke andar ek **Built-in System Directory / Database** hota hai.
Aap isko database ki **"Pata Index Book"** samajh sakte hain. Yeh khud user ka data store nahi karta, balki yeh poore database ki metadata (maaloomaat) rakhta hai — maslan:
 * Is database mein kul kitni tables hain?
 * Un tables ke naam kya hain?
 * Un tables ke andar konse columns hain aur unki data types kya hain?
Aap information_schema.tables aur information_schema.columns ko query karke kisi bhi chhupe hue table ya column ka naam dhoond sakte hain.
**Oracle Ka Fark:** Oracle database mein information_schema nahi hota, wahan is kaam ke liye all_tables aur all_tab_columns istemal hota hai.

### 2. Jab Naamaat Na Pata Hon Toh Andaza / Dhoondne Ka Tariqa
Jab aapko table aur column ke naam na pata hon, toh teen (3) tariqon se pata lagaya jata hai:
**A. Automatic / System Metadata Querying (Sub Se Secure Aur Confirm Tariqa):**
Aap direct information_schema se saare tables ke naam mangwa lete hain:
 1. **Tables Ke Naam Pata Karna:**
   ```sql
   ' UNION SELECT table_name, NULL FROM information_schema.tables--
   
   ```
 2. **Columns Ke Naam Pata Karna:**
   Jab table ka naam (maslan users_dev_v2) mil jaye, toh uske columns dhoondte hain:
   ```sql
   ' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users_dev_v2'--
   
   ```
**B. Wordlists / Brute-Forcing (Andaza Lagana):**
Jab information_schema ka access na ho, toh Wordlists (common names ki list) istemal ki jaati hai:
 * Tools (jaise Burp Suite Intruder ya SQLmap) ke zariye common words trial-and-error se bheje jaate hain: users, admin, accounts, members, login_data wagairah.
 * Agar payload error na de, toh woh naam exist karta hai.
**C. Application Context / Intuition:**
Tester website ke function se andaza lagata hai:
 * Login page par test kar rahe hain toh tables: users, accounts, admin, credentials.
 * E-commerce store par hain toh tables: products, orders, payments, cards.


