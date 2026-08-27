### Different Formats Mein SQL Injection Aur WAF Bypass Logic
SQL Injection sirf URL query string (jaise ?id=1) tak mehdood nahi hota. Input koi bhi ho sakta hai—bas shart yeh hai ke application us user input ko database SQL query mein process kar rahi ho. Modern web applications aksar data ko **JSON** ya **XML** format mein receive karti hain.
### Key Points Analysis
**1. Formats Change Hone Se Obfuscation Ka Mauqa:**
 * Jab data JSON ya XML format mein bhejte hain, toh Security Firewalls (WAF - Web Application Firewall) aam taur par basic SQL keywords (jaise SELECT, UNION, DROP) ko check karke request block kar dete hain.
 * Lekin agar developer ne XML ya JSON parser ko backend par sahi configure na kiya ho, toh aap in keywords ko **Encode / Escape** karke WAF ko ullu bana sakte hain.
**2. XML Encoding Bypass Example:**
 * Target payload: SELECT * FROM information_schema.tables
 * WAF SELECT keyword dekh kar block kar raha tha.
 * Developer ne XML format mein input liya:
   ```xml
   <stockCheck>
       <productId>123</productId>
       <storeId>999 &#x53;ELECT * FROM information_schema.tables</storeId>
   </stockCheck>
   
   ```
 * **Bypass Execution Flow:**
   1. **WAF Layer:** Firewall request ko dekhta hai. Usko SELECT nazar nahi aata kyunki wahan &#x53; (jo ke Hex XML Escape Entity hai letter 'S' ke liye) likha hua hai. WAF request ko safe samajh kar jaane deta hai.
   2. **XML Parser Layer:** Backend Application pehle XML ko Decode karti hai, jis se &#x53; wapas **S** ban jata hai.
   3. **Database Layer:** Query decode ho kar full SELECT * FROM... ban kar SQL Engine ke paas chali jati hai aur execute ho jati hai.

### Core Insight
WAF request ko **raw text** ki tarah inspect kar raha hota hai, jabke Web Server database query chalane se **pehle decode** kar deta hai. Is WAF vs Parser ke difference ka fayda utha kar filtering bypass ki jati hai.
