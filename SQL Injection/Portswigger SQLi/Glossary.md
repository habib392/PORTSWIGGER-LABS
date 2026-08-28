### SQL Injection (SQLi) Comprehensive Attack Glossary
 * **In-Band SQLi (Classic SQLi):**
   Jab attacker ka injected payload aur database ka output/result bilkul same communication channel (yani immediate HTTP response page) mein nazar aa jaye. Is ki do main types hoti hain:
   * **In-Band Error-Based SQLi:** Database ke verbose error messages (jaise SQL syntax error ya CAST() conversion errors) ka faida utha kar intentional errors trigger kiye jaate hain jin ke andar sensitive data return ho kar screen par print ho jata hai.
   * **In-Band UNION-Based SQLi:** Original query ke aage UNION SELECT operator ko append karke additional queries execute ki jati hain taake application ke normal response screen par kisi doosre table (jaise users) ka data merge kar ke display karwaya ja sake.
 * **Blind SQL Injection:**
   Jab application SQL execution ke direct results, error messages, ya dynamic data screen par output nahi karti, lekin backend par injected query database runtime behavior ko impact karti hai. Is ki main types niche hain:
   * **Blind SQLi with Conditional Responses:** Injection ki wajah se response body mein subtle difference aata hai (maslan page par *"Welcome back"* ka show hona ya na hona). Attacker Boolean logic (TRUE/FALSE statements) se character-by-character data brute-force karta hai.
   * **Blind SQLi with Conditional Errors:** Application status code ya error message hide kar leti hai, lekin SQL execution level par dynamic error (jaise divide-by-zero ya 1/0) generate kar ke conditional behavior detect kiya jata hai.
   * **Time-Based Blind SQLi:** Jab page ka textual content bilkul static rahe, toh database sleep/delay functions (jaise pg_sleep(), WAITFOR DELAY, dbms_pipe.receive_message) inject kar ke backend response time ke difference se Boolean query True/False evaluate ki jati hai.
 * **Out-of-Band SQL Injection (OAST):**
   Jab target application response body, error messages, ya response time mein koi change nahi dikhati (asynchronous/background process processing). Attacker database-specific network/file functions (maslan MS SQL server mein master..xp_dirtree, Oracle mein UTL_INADDR, ya PostgreSQL mein COPY) use karke database server se apne control mein maujood external DNS/HTTP listener (jaise Burp Collaborator ya ProjectDiscovery Interactsh) par request trigger karwaata hai aur direct subdomains concatenate karke data exfiltrate karta hai.
 * **Second-Order SQL Injection (Stored SQLi):**
   Jab attacker ka malicious input initial HTTP request par directly query execute nahi karta balki safely database mein store ho jata hai (Step 1). Baad mein jab koi doosra handler ya feature us stored data ko database se retrieve karke kisi aur internal SQL query mein unsanitized concatenation ke sath run karta hai, tab attack execute hota hai (Step 2).
 * **SQLi via Alternative Contexts & Formats:**
   Standard URL parameters ya GET query string ke ilawa data formats (jaise XML payload structure ya JSON API endpoints) ke andar SQL syntax inject karna. Is context mein XML Hex Entities (&#x53;) ya Unicode encoding se security filters aur Web Application Firewalls (WAFs) ko bypass kiya jata hai.
### Core Technical Techniques & Helper Clauses
 * **Database Column Enumeration:** ORDER BY 1, ORDER BY 2... ya UNION SELECT NULL, NULL ka use karke original SQL query ke exact column count ko hit-and-try method se identify karna.
 * **Data Type Compatibility Probe:** Columns count confirm hone ke baad NULL values ki jagah specific data types (maslan SELECT 'a', NULL) bhej kar string-compatible column position detect karna.
 * **Database Version Enumeration:** Universal system functions aur tables (jaise PostgreSQL ka version(), MS SQL ka @@version, Oracle ka FROM v$version, ya MySQL ka @@version) se database engine aur platform scan karna.
 * **Information Schema Exploitation:** Relational databases ke systematic metadata tables (jaise information_schema.tables aur information_schema.columns) ko query kar ke environment mein exist karne waale tamam custom tables aur columns ke naam map karna.
 * **String Concatenation Mechanics:** Data extract karte waqt database-specific syntax (PostgreSQL/Oracle mein ||, MS SQL mein +, aur MySQL mein CONCAT()) ka istemal karke multiple fields (username + password) ko Single Column output format mein combine karna.
