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

---

### 1. Check Stock Feature Aur Iski Availability
 * **Konsi Websites Mein Hota Hai?:** Yeh feature E-commerce websites (maslan Amazon, Daraz, Shopify stores, ya electronics ki online shops) par hota hai. Jab aap koi product khareedne lagte hain, toh "Check Stock" ya "Availability in Store" par click karke dekhte hain ke falan city ya warehouse mein is item ke kitne units baqi hain.
 * **Kya Aaj Bhi Exist Karta Hai?:** Bilkul, yeh feature har bari E-commerce site par aaj bhi active hota hai, kyunki inventory manage karna store ki zaroorat hoti hai.
### 2. Is Option Se Username / Password Kaise Nikla?
Aap ne sahi socha—pehle database ki structure samajhna zaroori hai. System backend par real query chala raha hota hai:
```sql
SELECT stock_count FROM store_inventory WHERE store_id = 1;

```
#### Step A: Columns Count aur Database Errors Detect Karna
Pehle tester check karta hai ke query kitne columns return karti hai.
 * Input: 1 UNION SELECT NULL \rightarrow Page normal response deta hai (Stock: 0 ya valid number). Is se confirm hota hai ke original query **1 hi column** return kar rahi hai.
 * Agar hum 1 UNION SELECT NULL, NULL bhejein ge, toh database crash ho jayega kyunki columns match nahi honge.
#### Step B: Tables Aur Password Extract Karna
Jab column 1 confirm ho jaye, toh tester standard tables query karta hai (ya guessing karta hai kyunki 90% web apps mein user table ka naam users aur columns username, password hi hotay hain):
```sql
1 UNION SELECT username || '~' || password FROM users

```
 * Key point: Kyunki column sirf **1** hi allowed tha, is liye PostgreSQL/SQLite ke concatenation operator (||) se username aur password ko ~ symbol ke zariye aapas mein jod kar single text format banaya gaya.
 * Result mein database ne stock count ki jagah screen par print kar diya: administrator~v3rYs3cur3P@ss.
### 3. XML Hex Encoding Kaise Karte Hain? (Step-by-Step)
Hex Entities Encoding ka matlab hota hai text ke har character ko uske Hexadecimal ASCII code mein convert karke XML entity Format &#xHEX; mein likhna.
**Misaal (Word "SELECT"):**
 * Letter **S** ka ASCII Hex Code = 53 \rightarrow XML Format: &#x53;
 * Letter **E** ka ASCII Hex Code = 45 \rightarrow XML Format: &#x45;
 * Letter **L** ka ASCII Hex Code = 4c \rightarrow XML Format: &#x4c;
 * Letter **E** ka ASCII Hex Code = 45 \rightarrow XML Format: &#x45;
 * Letter **C** ka ASCII Hex Code = 43 \rightarrow XML Format: &#x43;
 * Letter **T** ka ASCII Hex Code = 54 \rightarrow XML Format: &#x54;
**Kahan Se Convert Karein?:**
 1. **Online Tool:** Aap CyberChef (cyberchef.org) par ja kar text daal kar "To Hex" karke aage &#x prefix laga sakte hain.
 2. **Python Script:** Python terminal mein ek line ke code se convert kar sakte hain:
   ```python
   payload = "1 UNION SELECT username || '~' || password FROM users"
   encoded = "".join([f"&#x{ord(c):x};" for c in payload])
   print(encoded)
   
   ```
### 4. XML Kya Cheez Hai Aur Isme Konsi Language Use Hoti Hai?
 * **XML Kya Hai?:** XML ka matlab **Extensible Markup Language** hai. Yeh HTML ki tarah hi dikhti hai (tags <tag>data</tag> hotay hain), lekin HTML ka kaam page ko browser par *show* karna hota hai, jabke XML ka kaam 2 alag systems ya servers ke beech **data transfer / store** karna hota hai.
 * **Konsi Language Use Hoti Hai?:** XML khud koi programming language nahi hai, yeh ek **Data Format (Markup Language)** hai.
 * **Backend Integration:** Server-side par backend language (Python, Java, PHP, C#) is XML data ko parse (read) karke SQL database query banati hai.
### 5. Kya XML / JSON Ke Ilawa Aur Formats Bhi Hotay Hain?
Ji haan, web applications mein client aur server ke beech communication ke liye yeh formats use hotay hain:
 * **JSON (JavaScript Object Notation):** Aaj kal 80% APIs aur mobile apps isko use karti hain (e.g., {"storeId": "1"}).
 * **XML:** Enterprise systems, banking portals, aur purani SOAP APIs mein bohot zyada chalta hai.
 * **GraphQL:** Modern web applications mein API queries ke liye use hota hai.
 * **YAML:** Microservices aur configuration pipelines mein testing ke dauran use hota hai.
 * **Multipart / Form-Data & URL-Encoded:** Standard HTML forms ka input data format.
### 6. Real-World Rarity (Kya Aaj Bhi Milti Hai?)
 * **WAF Bypass via XML/JSON Encoding:** **Medium to Common.** Modern WAFs (jaise Cloudflare, AWS WAF) standard URL payloads ko fawran rok dete hain. Lekin jab data XML ya JSON body ke andar Unicode, Hex, ya UTF-16 encoding mein bheja jata hai, toh WAFs aksar confuse ho jate hain kyunki WAF raw body padhta hai jabke backend application use decode kar leti hai.
 * **Overall Rating:** Bug bounty mein aisi WAF bypass vulnerabilities bohot valuable hoti hain aur in par ache bounties milte hain.
