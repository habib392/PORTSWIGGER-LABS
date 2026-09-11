Jab real-world website par aap SQL Injection test karte hain aur wahan koi visual error display nahi hota (página bilkul normal reload hoti rehti hai), toh is situation ko Web Security mein **"Blind SQL Injection"** aur **"Silent Filter / Out-of-Band Handling"** kehte hain.

Is scenario ko handle karne aur check karne ki exact methodology step-by-step yeh hai:

### 1. Differential Analysis (Boolean-Based Test)
Sab se pehle check karein ke kya application True aur False response mein minor difference dikha rahi hai ya nahi.
 * **True Query:** Input mein 1 AND 1=1 ya 1' AND '1'='1 bhejen.
 * **False Query:** Input mein 1 AND 1=2 ya 1' AND '1'='2 bhejen.
**Kya Dekhna Hai?**
Page ka text error nahi dega, lekin:
 * Kya HTTP Status Code alag hai? (e.g., True par 200 OK, False par 500 ya 404)
 * Kya Content-Length (response size) mein thoda sa farq hai?
 * Kya page par koi element hide/show ho raha hai (jaise "Welcome User" ghayab ho jaye)?
Agar True aur False query mein farq nazar aaye, toh is ka matlab hai ke **Boolean Blind SQLi** maujood hai.
### 2. Time-Based Blind Testing (Sub-surface Validation)
Agar application True aur False dono par **exact same response** (normal page) de rahi hai, toh aap **Time-Based Injection** try karte hain. Is mein hum database ko command dete hain ke agar vulnerability hai toh response late kare.
 * **MySQL Payload:** 1 AND SLEEP(5)# ya 1' AND SLEEP(5)-- -
 * **PostgreSQL Payload:** 1'; SELECT pg_sleep(5);-- -
**Result Identification:**
 * Agar page normal milliseconds ke bajaye **5 seconds ke delay** ke baad load hota hai, toh confirmed Injection confirmed hai, chahe samne koi error aaye ya na aaye.
### 3. Out-of-Band (OAST) Technique
Agar target Application input ko Background Async Request ya Queue mein process kar rahi hai (jahan response screen par kabhi nahi aata), toh tester **Out-of-Band (OAST)** technique use karta hai.
 * Burp Suite ka **Burp Collaborator** Tool use kiya jata hai.
 * Payload ke andar DNS/HTTP request trigger ki jati hai apne Collaborator server par (e.g., LOAD_FILE() ya master..xp_dirtree ke zariye).
 * Agar aap ke Burp Collaborator par HTTP/DNS ping aa jaye, toh payload execution confirm ho jati hai.
### 4. Input Silently Dropped / Sanitized (No Vulnerability)
Agar aap ne Boolean, Time-Based (SLEEP), aur OAST tamaam tests perform kar liye aur:
 * Response Time 0 seconds hi raha,
 * Response Content-Length bilkul identical raha,
 * Collaborative ping nahi aaya,
Toh is ka matlab hai ke application backend par Prepared Statements / Parameterized Queries (PDO ya PreparedStatement) use kar rahi hai, aur aap ka input code ka hissa banne ke bajaye purely as Data process ho raha hai. Is ka matlab website us endpoint par fully secure hai!
