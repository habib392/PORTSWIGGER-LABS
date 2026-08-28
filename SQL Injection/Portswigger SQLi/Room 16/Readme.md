### Second-Order (Stored) SQL Injection Kya Hai?
**First-Order SQL Injection:**
Jab aap application ko koi input bhejte hain aur woh fawran ushi HTTP request mein unsafe tareeqay se SQL query mein chala jata hai aur attack perform ho jata hai (jaise ab tak hum pichli labs mein karte aaye hain).
**Second-Order SQL Injection:**
Yeh attack **2 alag alag steps** mein hota hai:
 * **Step 1 (Input Store Hona):** Aap application ko malicious input bhejte hain. Developer yahan par input ko safe tareeqay se (maslan Prepared Statements se) **database mein store** kar leta hai. Yahan koi attack execute nahi hota aur na hi koi error aata hai.
 * **Step 2 (Data Trigger / Execution):** Baad mein jab aap koi *doosri* HTTP request bhejte hain ya koi doosra page open karte hain, toh application database se wahi store kiya hua data nikal kar kisi *doosri* SQL query mein unsafe (concatenate) karke chala deti hai. Is point par attack execute ho jata hai.
### Developer Ki Sab Se Bari Ghalat-fehmi
Second-Order SQL Injection zyadatar is wajah se paida hota hai kyunki developer yeh samajhta hai:
> *"Jo data mere apne database se aa raha hai, woh toh bilkul safe aur trusted hai, us se mujhe koi khatra nahi ho sakta."*
> 
Developer input ko database mein save karte waqt toh sanitize kar leta hai, lekin jab database se nikal kar kisi doosri query mein use karta hai, toh filtering bhool jata hai.
### Simple Real-World Misaal (User Profile & Password Reset)
 1. **Account Registration (Step 1):**
   * Aap naya account banate waqt apna username rakhte hain: admin'--
   * Application is username ko safely database mein save kar leti hai.
 2. **Password Change Page (Step 2):**
   * Aap login karke apna password change karte hain.
   * Backend par developer yeh vulnerable query chalata hai:
     ```sql
     UPDATE users SET password = 'new_password' WHERE username = 'admin'--'
     
     ```
   * Database se nikalne wala admin'-- query ko truncate kar deta hai. Nateeja yeh hota hai ke real administrator user ka password change ho kar aapka wala password ban jata hai!
