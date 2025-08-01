**File upload vulnerabilities kya hoti hain?**

Dost, file upload vulnerabilities tab hoti hain jab koi website users ko files upload karne deti hai, lekin un files ka naam, type, content ya size properly check nahi karti. Is wajah se, ek simple image upload function bhi dangerous files, jaise server-side scripts, upload karne ke liye use ho sakta hai, jo remote code execution ka sabab ban sakta hai.

**Ye vulnerabilities kyun aati hain?**

Aksar developers sochte hain ke unhone strong validation laga di, lekin wo kaafi nahi hoti ya bypass ho jati hai. Misal ke taur pe, wo dangerous file types ko blacklist karte hain, lekin file extensions check karte waqt chhoti si galti kar dete hain. Ya phir, wo file type ko check karne ke liye aisi properties dekhte hain jo attackers tools (jaise Burp Proxy) se asani se change kar sakte hain.

**Inka impact kya hota hai?**

Impact do cheezon pe depend karta hai:
1. Website file ke kis hisse ko properly validate nahi karti (size, type, content, etc.).
2. Upload hone ke baad file pe kya restrictions hain.

Sab se bura scene tab hota hai jab file type check nahi hota aur server kisi file (jaise .php ya .jsp) ko code ki tarah execute karta hai. Aisa hone pe attacker ek web shell upload kar sakta hai, jo usay server ka full control de deta hai. Agar filename check nahi hota, to attacker important files overwrite kar sakta hai. Ya agar file size check nahi hoti, to attacker disk space bhar ke denial-of-service (DoS) attack kar sakta hai.

**Web servers static files ko kaise handle karte hain?**

Pehle websites mostly static files pe based hoti thi, jo user ke request pe server se bheji jati thi. Ab websites dynamic hain, lekin phir bhi static files (jaise images, stylesheets) serve hoti hain. Server request ke path se file extension check karta hai, phir usay MIME type se match karta hai. Agar file non-executable hai (jaise image), to server usay directly client ko bhej deta hai. Agar executable hai (jaise PHP) aur server usay execute karne ke liye configured hai, to wo script run karta hai. Agar executable file hai lekin execute nahi hoti, to server error deta hai, ya kabhi-kabhi file ka content plain text ke taur pe leak ho jata hai.

**Web shell kya hota hai aur isay kaise exploit karte hain?**

Web shell ek malicious script hota hai jo attacker ko server pe commands run karne deta hai bas HTTP requests bhej ke. Agar tum web shell upload kar lo, to server tumhare control mein aa jata hai. Tum files read/write kar sakte ho, sensitive data chura sakte ho, ya server ko dusre attacks ke liye use kar sakte ho.

Misal ke taur pe, ye simple PHP code server ke kisi bhi file ko read kar sakta hai:
```php
<?php echo file_get_contents('/path/to/target/file'); ?>
```
Is file ko upload kar ke, jab tum isay request karoge, to target file ka content response mein mil jayega.

**Kaise test karna chahiye?**

1. **File type check karo**: Website kaunsi file types allow karti hai? Kya .php, .jsp jaisi dangerous files upload ho sakti hain?
2. **Filename validation**: Kya tum same naam ki file upload kar ke overwrite kar sakte ho? Directory traversal possible hai?
3. **Size restrictions**: Kya bohot bari file upload kar ke server ka disk bhar sakte ho?
4. **Bypass tricks try karo**: File extension change karo (jaise .php ko .php.jpg), ya headers manipulate karo tools se.
5. **Tools use karo**: Burp Suite jaise tools se file upload requests ko intercept aur modify karo.
