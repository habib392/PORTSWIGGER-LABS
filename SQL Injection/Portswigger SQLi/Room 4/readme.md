### Database Ke Mutabiq Syntax (Fark / Differences)
Alag alag databases (jaise Oracle, MySQL) mein SQL injection ke syntax mein thora fark hota hai:
**1. Oracle Database Ki Shart (DUAL Table):**
Oracle database mein har SELECT query ke saath FROM aur ek valid table ka naam likhna lazmi hota hai.
Is maqsad ke liye Oracle mein ek built-in table hoti hai jise **DUAL** kehte hain.
Toh Oracle par aapka UNION payload aisa hoga:
```sql
' UNION SELECT NULL FROM DUAL--

```
**2. Comment Sequences Ka Fark:**
 * **MySQL:** MySQL mein double-dash (--) ke baad **ek space** hona zaroori hai (yani -- ). Agar space nahi hoga toh comment kaam nahi karega. Iske alawa MySQL mein # (hash) ka nishan bhi comment ke liye istemal hota hai.
 * **Doosre Databases (SQL Server, PostgreSQL, etc.):** In mein aam tor par bina space ke bhi -- kaam kar jata hai.

---

### Kaam Ki Data Type Wale Columns Dhoondna
UNION attack ke zariye jo data hum nikalna chahte hain (jaise usernames, passwords, API keys), woh aam tor par **String (Text)** ki shakl mein hota hai. Is liye hume yeh pata lagana hota hai ke original query ke kaun se columns Text data ko support karte hain.
Jab aapko total columns ki ginti (number of columns) pata chal jaye, toh aap ek ek karke har column mein String ('a') daal kar check karte hain.
**Payloads Ki Misaal (Agar 4 Columns Hain):**
```text
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--

```
**Result Aur Response Kaise Pehchanein?**
 1. **Incompatible / Galat Data Type:** Agar woh column String ko support nahi karta (maslan woh Integer/Number column hai), toh database error de dega.
   * *Error Misaal:* "Conversion failed when converting the varchar value 'a' to data type int."
 2. **Compatible / Sahi Data Type:** Agar koi error nahi aata aur application ke response/page par aapko woh string (yani 'a') likha hua nazar aa jata hai, toh iska matlab hai ke yeh column Text data nikalne ke liye bilkul sahi hai.

