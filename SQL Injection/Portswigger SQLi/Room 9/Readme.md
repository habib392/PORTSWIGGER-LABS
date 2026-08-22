### Blind SQL Injection Kya Hai?
Blind SQL Injection tab hoti hai jab application mein SQL Injection ki vulnerability (khami) toh hoti hai, lekin database ka **data ya errors screen par/HTTP response mein nazar nahi aate**.

UNION attacks jaisi techniques yahan **kaam nahi karti**, kyunki un attacks ke liye zaroori hota hai ke aap injected query ka result screen par dekh sakein.

Halanke data screen par dikhai nahi deta, phir bhi alag alag techniques (jaise application ke behavior, Boolean logic, ya Time Delays) ka istemal karke secret data ko nikalna mumkin hota hai. Isko hum kehte hain ke database se "andhere mein" ya "andhe ho kar" data nikalna.

---

### Conditional Responses Ke Zariye Blind SQL Injection Exploitation
Jab website screen par database ka data ya error nahi dikhati, toh hum application ke **behavior (vartaao/response)** mein aane wale badlaao se andaza lagate hain.
**Misaal Ke Tor Par:**
Website user ko track karne ke liye cookie bhejti hai:
Cookie: TrackingId=u5YD3PapBcR4lN3e7Tj4
Backend par yeh SQL query chalti hai:
```sql
SELECT TrackingId FROM TrackedUsers WHERE TrackingId = 'u5YD3PapBcR4lN3e7Tj4'

```
**Application Ka Behavior (Response):**
 * **Agar TrackingId Sahi Hai (Data Mila):** Page par *"Welcome back"* ka message aata hai.
 * **Agar TrackingId Galat Hai (Data Nahi Mila):** Page normal load hota hai lekin *"Welcome back"* ka message nahi aata.
**Is Behavior Ka Fayda Kaise Uthayein?**
Hum true/false (Haan ya Naa) wale conditions inject karte hain:
 * **True Condition (' AND 1=1--):** Response mein *"Welcome back"* dikhai dega.
 * **False Condition (' AND 1=2--):** Response mein *"Welcome back"* nahi aayega.
Isi Haan/Naa (True/False) logic ko istemal karke hum ek ek character karke passwords aur doosra secret data nikal sakte hain.

---

### Conditional Responses Ka Practical Exploitation
Is concept ko samajhne ke liye maan lein ke hum do (2) alag alag queries TrackingId cookie mein bhejte hain:
 1. **Pehla Request (...xyz' AND '1'='1):**
   * Yeh condition **True** (Sahi) hai.
   * Database matching data return karega aur screen par **"Welcome back"** likha hua nazar aayega.
 2. **Dusra Request (...xyz' AND '1'='2):**
   * Yeh condition **False** (Galat) hai.
   * Database koi data return nahi karega, is liye screen par **"Welcome back"** likha hua **nahi** aayega.
**Nateeja (Key Takeaway):**
Is True/False tariqay se hum database se koi bhi True/False sawal pooch sakte hain (maslan: *"Kya admin password ka pehla letter 'a' hai?"*). Agar "Welcome back" aaya toh matlab Haan (True), agar nahi aaya toh matlab Naa (False). Is tarah ek ek character karke poora data bahar nikala ja sakta hai.

---

### One-by-One Character Pata Lagana (Character Extraction)
Maan lein ke database mein Users table hai jisme Username aur Password columns hain, aur hume Administrator ka password nikalna hai. Hum ek ek character karke test karenge.
Iske liye hum **SUBSTRING** function ka istemal karte hain jo text mein se specific position ka character alag karke check karta hai.
**Step-by-Step Testing Process:**
**1. Pehla Test (> 'm' Check Karna):**
```sql
xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 'm

```
 * **Matlab:** Kya password ka 1st character 'm' se bara hai?
 * **Result:** Screen par "Welcome back" **aaya** (Condition True). Matlab character 'm' se aage ka koi letter hai (maslan n, o, p, q, etc.).
**2. Dusra Test (> 't' Check Karna):**
```sql
xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 't

```
 * **Matlab:** Kya password ka 1st character 't' se bara hai?
 * **Result:** Screen par "Welcome back" **nahi aaya** (Condition False). Matlab character 't' se bara nahi hai.
**3. Teesra Test (= 's' Confirm Karna):**
```sql
xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) = 's

```
 * **Matlab:** Kya 1st character exact 's' hai?
 * **Result:** Screen par "Welcome back" **aaya** (Condition True). Confirm ho gaya ke password ka pehla letter **s** hai!
**Next Steps:**
Isi tarah hum position change karte jayenge (2, 1 doosre letter ke liye, 3, 1 teesre ke liye) jab tak poora password tayyar na ho jaye.

**Khas Note:**
Kuch databases (jaise Oracle) mein SUBSTRING ki jagah **SUBSTR** likha jata hai.

