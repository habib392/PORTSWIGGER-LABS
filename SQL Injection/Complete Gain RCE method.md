Step 1: Initial Fuzzing & Anomaly Detection
Aap ne jo pehla step bataya, wahi se shuruaat hoti hai. Har entry point (URL parameter, search box, login form) par generic payloads (', ", \, OR 1=1) inject karke check karte hain ke application ka response change hota hai ya nahi (jaise error aana, page blank hona, ya delay aana).
​Step 2: Vulnerability Type & Behavior Identification
Response analyze karne ke baad decide karte hain ke injection kis category ki hai:
​Error-Based: Agar screen par SQL error direct show ho jaye.
​In-Band (UNION-based): Agar original data ke sath apne custom data ke columns display karwaye ja sakein.
​Blind (Boolean/Time-based): Agar koi error ya data show na ho, toh True/False conditions ya SLEEP() function ke zariye page ka behavior observe kiya jaye.
​Step 3: Database Enumeration (Information Gathering)
Query ka structure samajhne ke baad database ke andar ki maloomat nikalni hoti hain taake pata chale system kiske haath mein hai:
​Current database ka naam maloom karna (SELECT database()).
​Database ka version aur logged-in user check karna (SELECT version(), user()).
​Tables aur columns ke naam extract karna (Information schema queries ke zariye).
​Step 4: Privileges & Permissions Check
RCE ya file writing tak pohanchna hai toh yeh dekhna lazmi hai ke current database user ke paas kya powers hain:
​Check karna ke user root ya DBA (Database Administrator) hai ya nahi.
​Database configuration check karna (jaise MySQL mein secure_file_priv ki setting dekhna taake file export allow ho).
​Step 5: File Writing & Web Shell Injection
Agar permissions allow karein aur folder writeable ho, toh SQL query ke zariye server ke web root folder mein ek command execution script (web shell) write ki jati hai:
​Payload structure: UNION SELECT 1, '<?php system($_GET["cmd"]); ?>' INTO OUTFILE '/path/to/webroot/shell.php'
​Step 6: Executing OS Commands (Achieving RCE)
Akhri step mein, jo web shell file server par save ki gayi hai, usko browser ya tool ke zariye call karke underlying operating system par commands chalayi jati hain:
​URL access: http://<target-ip>/path/to/shell.php?cmd=whoami
​Agar output mein server user (www-data ya root) nazar aa jaye, toh iska matlab hai ke SQL Injection se kamyabi ke sath Remote Code Execution (RCE) hasil ho chuka hai.