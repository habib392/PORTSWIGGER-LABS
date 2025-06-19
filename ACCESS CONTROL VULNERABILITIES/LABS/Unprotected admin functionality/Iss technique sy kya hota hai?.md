# 🔑 Technique ka naam:
robots.txt enumeration

(yaani robots.txt file ko check kar ke hidden paths nikalna)

### 🎯 Iska faida penetration testing mein kya hai?
Admin ya private pages ka location mil jata hai

Jaise: /administrator-panel, /backup, /config, etc.

Unprotected functionalities discover ho jaati hain

Agar admin page pe authentication nahi lagi hui, to tum sidha access le sakte ho.

Information disclosure detect kar sakte ho

robots.txt mein sensitive URLs disclose karna ek misconfiguration hai.

### 🧠 Real-world example:
Agar koi ecommerce site ho aur robots.txt mein likha ho:

**Disallow: /admin
Disallow: /dev-notes.txt**

To yeh attacker ke liye treasure ban jaata hai! Admin panel aur developer notes mil gaye bina kisi effort ke.

### 🔒 Defense (developer kya kare):

robots.txt mein sirf non-sensitive pages ka mention ho.

Admin ya sensitive areas authenticate hone chahiye, chahe URL kisi ko maloom bhi ho.

URLs guess-proof aur obfuscated banayein (/secure-panel-2x3yz/ etc.)



