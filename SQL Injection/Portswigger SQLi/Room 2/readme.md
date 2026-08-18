### Application Logic Ko Bypass Karna
Farz karein ek login system hai jo username aur password check karne ke liye backend par yeh SQL query chalata hai:
```sql
SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'

```
Agar details sahi hon toh login ho jata hai, warna reject.
Attacker bina password ke kisi bhi account (jaise administrator) mein login karne ke liye SQL comment -- ka istemal karta hai.
Jab attacker username mein administrator'-- daalta hai aur password blank chhodta hai, toh query yeh ban jati hai:
```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''

```
Yahan -- ke baad wala poora hissa (AND password = '') comment ban kar ignore ho jata hai. Database sirf administrator ka username check karta hai aur bina password ke attacker ko login kar deta hai.

