### Time Delays Ke Zariye Blind SQL Injection Exploitation

Agar application SQL queries ke waqt aane wale sabhi database errors ko catch (pakad) kar le aur unhein gracefully handle kare (yani screen par na error aaye aur na koi response change ho), toh pichli conditional error wali technique yahan kaam nahi karegi.

Is soorat-e-hal mein hum SQL query ke andar **Time Delays (Waqt Ka Waqfa)** trigger karwate hain:

* **Logic:** Hum query ko batate hain ke agar hamaari condition True ho, toh database execution ko kuch seconds ke liye rok de (sleep karwa de).
* **Application Behavior:** Kyun ke web applications aam tor par queries ko synchronous process karti hain (yani jab tak database query khatam na ho, response wapas nahi bheja jata), is liye database roknay se poori HTTP response delay ho jati hai.
* **Nateeja:**
* Agar response aane mein delay hua (maslan page load hone mein 10 second lage), toh condition **True** thi.
* Agar response instantly (1 second mein) aa gaya, toh condition **False** thi.

---

### Time Delays Ka Practical Logic (MS SQL Server Misaal)

Time delays trigger karne ke tareeqay har database type ke hisaab se alag hotay hain. Misaal ke taur par, **Microsoft SQL Server (MS SQL)** par hum `WAITFOR DELAY` ka istemal karte hain:

**1. Pehla Test (False Condition):**

```sql
'; IF (1=2) WAITFOR DELAY '0:0:10'--

```

* **Logic:** Condition `1=2` **False** hai.
* **Result:** Database delay trigger nahi karega aur response **instantly (fawran)** wapas aa jaye ga.

**2. Dusra Test (True Condition):**

```sql
'; IF (1=1) WAITFOR DELAY '0:0:10'--

```

* **Logic:** Condition `1=1` **True** hai.
* **Result:** Database query execute karte waqt **10 seconds tak ruk (wait) jaye ga**, jis se web response aane mein exact 10 seconds ka delay aaye ga.

---

### Time Delays Se Password Extraction

Isi delay wali logic ko istemal karke hum password ka ek ek character test kar sakte hain:

```
'; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') = 1 WAITFOR DELAY '0:0:10'--

```

* **Server Response Decoding:**
* **Agar Response Mein 10 Sec Ka Delay Aaya:** Condition True thi — matlab 1st character `'m'` se bara hai.
* **Agar Response Instantly (1 Sec Mein) Aaya:** Condition False thi — matlab 1st character `'m'` se bara nahi hai.



**Khas Note:**
Microsoft SQL Server par `WAITFOR DELAY` chalta hai, jabke doosray databases par alag functions hotay hain (jaise PostgreSQL par `pg_sleep(10)` aur MySQL par `SLEEP(10)`).
