### SQL Injection Attacks Mein Database Ko Examine Karna (Jaanchhna)
SQL injection vulnerabilities ko poori tarah exploit karne ke liye, aksar database ke baray mein maloomat (information) dhoondna zaroori hota hai. Is maloomat mein do mukhya (main) cheezein shamil hain:
 1. **Database Software Ka Type Aur Version:** Yeh pata lagana ke backend par kaun sa database chal raha hai (maslan MySQL, PostgreSQL, Oracle, ya Microsoft SQL Server) aur uska version kya hai.
 2. **Database Ke Tables Aur Columns:** Yeh pata karna ke database ke andar kaun se tables hain aur un tables ke andar kaun se columns bane hue hain.

---

### Database Ki Type Aur Version Pata Karna
Aap alag alag databases ke specific queries (payloads) inject karke yeh pata laga sakte hain ke backend par kaun sa database chal raha hai aur uska version kya hai.
**Database Version Pata Karne Ki Commands:**
| Database Type | Query Syntax |
|---|---|
| **Microsoft (MSSQL) & MySQL** | SELECT @@version |
| **Oracle** | SELECT * FROM v$version |
| **PostgreSQL** | SELECT version() |
**Misaal (UNION Attack Ke Zariye):**
Aap yeh payload bhej sakte hain:
```sql
' UNION SELECT @@version--

```
**Result / Output:**
Agar response mein aisa text likha hua aaye:
> *Microsoft SQL Server 2016 (SP2) (KB4052908) - 13.0.5026.0 (X64) ...*
> 
Toh is se confirm ho jata hai ke database **Microsoft SQL Server** hai aur uska version **2016** hai.

