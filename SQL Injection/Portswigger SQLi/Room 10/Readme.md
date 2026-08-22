### Error-based SQL Injection Ka Urdu Tarjuma

Here is the simple and clear translation:

**Error-based SQL injection Kya Hai?**

Error-based SQL injection se murad aise cases hain jahan aap database ke **Error Messages** ka istemal karke sensitive data extract (bahar nikal) sakte hain ya uska andaza laga sakte hain — yahan tak ke blind scenarios mein bhi. Iske imkanat database ki configuration aur paida hone wale errors par depend karte hain:

* **Conditional Errors:** Aap application se kisi Boolean Expression (True/False) ke nateeje ki bunyad par ek khas error response paida karwa sakte hain. Isko aap bilkul waise hi exploit kar sakte hain jaise humne pichle section mein Conditional Responses ko dekha tha.
* **Verbose Error Messages:** Aap aise error messages trigger kar sakte hain jo query ke return shuda data ko error ke andar hi screen par print (output) kar dete hain. Yeh cheez blind SQL injection vulnerability ko visible (nazar aane wali) vulnerability mein badal deti hai.

---

### Conditional Errors Ke Zariye Blind SQL Injection Exploitation

Kuch applications SQL queries toh chalati hain, lekin unka behavior bilkul change nahi hota—chahe query data return kare ya na kare (yani *"Welcome back"* jaisa koi message nahi aata). Is wajah se pichli wali technique (Conditional Responses) yahan kaam nahi karti.

Aisi jagah par hum application se **SQL Error** paida karwa kar different response hasil karte hain:

* **Logic:** Hum query ko is tarah modify karte hain ke database **sirf tab hi error de jab hamaari condition True ho**.
* **Application Behavior:** Jab database error throw karta hai, toh server aksar ek alag response deta hai (maslan `500 Internal Server Error` ya blank page).
* **Nateeja:** Agar server par Error aaya, toh matlab hamaari condition **True** thi. Agar normal page load hua, toh matlab condition **False** thi.

---


### Conditional Errors Ka Practical Logic (Division by Zero)

Is technique ko samajhne ke liye dekhein ke jab hum `CASE` statement ka istemal karte hain toh error kaise trigger hota hai.

**1. Pehla Test (False Condition):**

```sql
xyz' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a

```

* **Logic:** Condition `1=2` **False** (galat) hai.
* **Result:** `CASE` statement `'a'` return karega. Math error (`1/0`) run nahi hoga, is liye koi error nahi aayega aur page **normal load** hoga.

**2. Dusra Test (True Condition):**

```sql
xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a

```

* **Logic:** Condition `1=1` **True** (sahi) hai.
* **Result:** Database `1/0` (divide by zero) perform karne ki koshish karega jo ke ek illegal math execution hai. Is se database mein **Error** paida hoga aur page par `500 Internal Server Error` show hoga.

---

### Password Character Extraction Expression

Isi logic ko istemal karke hum password ka ek ek character check kar sakte hain:

```sql
xyz' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a

```

* **Server Response Decoding:**
* **Agar Server Error Aaya (500 Status):** Matlab condition True thi — 1st character `'m'` se bara hai.
* **Agar Normal Response Aaya (200 Status):** Matlab condition False thi — 1st character `'m'` se bara nahi hai.



**Khas Note:**
Divide-by-zero (`1/0`) ke ilawa bhi alag alag databases par specific functions (jaise Oracle par `TO_NUMBER('abc')`) ke zariye errors trigger karwaye ja sakte hain.

---

Real-world web applications par har jagah `"Welcome back"` jaisa clear message nahi milta. Real-world testing mein hum different application behavior aur side-channels par depend karte hain.

Aisi jagahon par 4 sab se common real-world signals dekhe jate hain:

---

### 1. HTTP Response Codes (Status Codes)

Real targets par sab se zyada **Status Codes** ka faraq dekha jata hai:

* **True Condition:** Server `200 OK` return karega (page normal open hoga).
* **False Condition:** Server `404 Not Found`, `403 Forbidden`, ya `500 Internal Server Error` return kar sakta hai.

### 2. Error-Based Indicators (Unhandled Exceptions)

Jab response ka text change nahi hota, toh hum deliberate syntax/logic error throw karwate hain (jaise Divide-by-Zero `1/0` ya invalid type cast):

* **True Condition:** Database crash hoga aur server `500 Internal Server Error` ya blank output dega.
* **False Condition:** Query safai se execute ho jayegi aur page `200 OK` ke sath normal load hoga.

### 3. Response Length & Time (Content-Length / Delay)

* **Content-Length Faraq:** Agar screen par text change na bhi ho, tab bhi response ke size mein 2-4 bytes ka faraq aa jata hai (maslan True par 5400 bytes, False par 5395 bytes).
* **Time-Based Delay:** Agar application koi error ya response difference dikhati hi nahi, toh hum SQL query mein sleep command daalte hain (maslan `pg_sleep(5)` ya `WAITFOR DELAY '0:0:5'`).
* **True Condition:** Web page load hone mein 5 second lagaye ga.
* **False Condition:** Web page instantly load ho jayega.



### 4. Out-of-Band (OAST / Burp Collaborator)

Jab application se koi response, error, ya time delay nahi milta, toh hum SQL query ke zariye database ko force karte hain ke woh hamaare DNS/HTTP server (jaise Burp Collaborator) par ek out-of-band request bhej kar interactive connection banaye.

---

Real-world penetration testing mein hum pehle status codes aur response length check karte hain, aur agar application bilkul silent ho toh **Time-Based Delays** aur **Out-of-Band (OAST)** attacks ka sahara lete hain.
