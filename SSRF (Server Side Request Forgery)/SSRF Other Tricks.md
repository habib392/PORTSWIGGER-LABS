### ✅ Real websites pe aur kya tricks chalti hain?

1. **DNS Rebinding**

   * Victim ko lagta hai request public domain pe ja rahi hai, lekin domain resolve ho jata hai private IP pe.
   * Example: `attacker.com` → resolves first to `1.2.3.4`, then next request pe `127.0.0.1`.

2. **Alternative IP notations**

   * `127.0.0.1` ko alag formats main likhna:

     * Decimal: `2130706433`
     * Octal: `0177.0.0.1`
     * Hex: `0x7f.0x00.0x00.0x01`
   * Ye filters ko confuse kar dete hain.

3. **Bypass via URL schemes**

   * Agar `http/https` filter hai, kabhi kabhi `file://`, `gopher://`, `dict://`, `ftp://` allowed hota.
   * Ye exploitation level ko next step le ja sakta (internal files read, RCE payloads).

4. **Header injection / CRLF tricks**

   * `http://127.0.0.1:80%0d%0aHost:evil.com` → kuch parsers host rewrite kar dete hain.

5. **Cloud Metadata Endpoints**

   * AWS: `http://169.254.169.254/latest/meta-data/`
   * GCP, Azure ke bhi apne endpoints hote hain.
   * Bohot real bug bounties yahin se nikle hain.

6. **SSRF → Pivot to Internal Services**

   * Real cases main SSRF use hota hai Redis, Memcached, ElasticSearch, MongoDB ko hit karne ke liye.
   * Matlab internal infra ka full scan ho sakta hai.

7. **Blind SSRF Detection**

   * Kabhi response dikhai nahi deta, to Burp Collaborator / Interactsh use kar ke DNS/HTTP hit confirm karte hain.

---

### 🔥 Kahaan se seekh sakte ho?

* **Bug bounty writeups** (HackerOne, Medium, Intigriti blog) → yahan tumhe naye-naye SSRF payloads aur real bypasses milte hain.
* **PayloadsAllTheThings (GitHub)** → is repo main SSRF ke dedicated payloads aur tricks list hain.
* **WebSecLabs / HackTheBox / TryHackMe** → PortSwigger se zyada practical labs dete hain.
* **OWASP SSRF Cheatsheet** → defense + bypass dono angles.

---

1. Lab solve karo (PortSwigger)
2. PayloadsAllTheThings check karo
3. Bug bounty writeup compare karo → nayi trick samajh ke notes banao
