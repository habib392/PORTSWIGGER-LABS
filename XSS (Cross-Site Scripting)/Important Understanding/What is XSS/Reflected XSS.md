# 💡 Reflected XSS 

Reflected XSS tab hota hai jab website kisi user ka diya hua data URL se leti hai, aur usi waqt bina check kiye usko page par wapas dikhati hai.

### 🧪 Example samjho:

Tumhari ek website ka search function hai. Tum search karte ho “gift”, tou URL ban jata hai:

**https://insecure-website.com/search?term=gift**

Aur page pe aise dikhta hai:

**<p>You searched for: gift</p>**

Ab attacker isi cheez ka faida uthata hai. Wo “gift” ki jagah JavaScript daal deta hai:

**https://insecure-website.com/search?term=<script>alert('XSS')</script>**

Ab page ka response hoga:

**<p>You searched for: <script>alert('XSS')</script></p>**

Yani ab jab koi bhi banda attacker ka diya hua link open karega, uska browser turant wo JavaScript chala dega — aur alert pop-up show hoga.

### 😈 Iska nuksan kya hai?

Agar victim user already logged in hai, tou attacker uski:

- Session cookies chura sakta hai

- Uske naam pe koi bhi action perform kar sakta hai

- Sensitive info access kar sakta hai

Sab kuch user ke browser main hota hai, aur victim ko shayad kuch pata bhi na chale.

#💣 Reflected XSS ka Asar

Agar attacker ka script victim ke browser mein chal jaye, tou uska matlab hai ke attacker us user ka pura control le sakta hai — jaise:

User ki tarah koi bhi action kar sakta hai (jaise delete karna, update karna, form submit karna)

Jo kuch user dekh sakta hai, attacker bhi dekh sakta hai

Jo data user change kar sakta hai, attacker bhi change kar sakta hai

Dusre users ke sath bhi malicious interaction kar sakta hai — jaise fake message ya attack bhejna

### 💌 Attack deliver kaise hota hai?

Attacker link bana kar:

Apni website par daal sakta hai

Kisi forum/post/comment jahan content allowed ho wahan chipka sakta hai

Victim ko email, tweet, WhatsApp ya kisi message ke through bhej sakta hai

Ye link targeted bhi ho sakta hai (jaise kisi admin ke liye) ya public bhi ho sakta hai (sab logon ke liye).


### 🔍 Reflected XSS vs Stored XSS

Reflected XSS mein attacker ko link send karna padta hai — yaani victim ko kisi na kisi tarah se us link pe click karwana hota hai.

Lekin Stored XSS mein attacker ka code website ke andar hi permanently hota hai — victim ko kuch click karne ki bhi zarurat nahi hoti.

📌 Is liye Reflected XSS ka impact thoda kam hota hai Stored XSS ke mukable mein.

# 🎯 Reflected XSS — Different Contexts Mein

Reflected XSS ki kai alag alag types hoti hain — aur yeh depend karta hai ke:

1. Website ka response mein attacker ka input kahaan reflect ho raha hai

2. Aur kis form mein reflect ho raha hai

### 📍 Kya matlab hai "kahaan reflect ho raha hai"?

Yani:

- Agar input HTML tag ke andar hai, tou alag payload chahiye

- Agar input JavaScript block ke andar hai, tou payload different hoga

- Agar input attribute (jaise src ya href) mein hai, tou payload or alag banega

Yeh location decide karti hai ke kaunsa payload kaam karega — aur vulnerability ka impact bhi is pe depend karta hai.

### 🛡️ Data par processing ka asar

Agar website tumhare input pe koi validation ya filtering lagati hai (jaise < ya script remove kar deti hai), tou tumhe smart payload banana padta hai — jo us filter ko bypass kare.

Is liye reflected XSS ka success context + filtering dono cheezon par depend karta hai.

# 🕵️‍♂️ Reflected XSS Dhoondhne Aur Test Karne Ka Tarika – Tumhari Zuban Main

### ✅ 1. Burp Suite Scanner ka Use

Agar tumhare paas Burp Suite Professional hai, tou uska scanner automatically reflected XSS vulnerabilities ko fast aur reliably dhoond leta hai.

Lekin agar manual testing kar rahe ho (jo seekhne ke liye best hai), tou ye steps follow karo:

### 🔍 2. Har Input Point Test Karo

App ke har jagah jahan user input jaa sakta hai — jaise:

- URL parameters (e.g. ?search=abc)

- POST body

- URL path (e.g. /profile/username)

- HTTP headers (jaise User-Agent, Referer)

Sab ko alag alag test karo, kyunke har jagah alag context hoti hai.

### 🧪 3. Random Value Inject Karo

Sab jagah random 8-character alphanumeric (jaise a7x9p3d2) value daalo — taake pata chale kaha reflect ho rahi hai.

Burp Intruder se aise random values automatically generate kar sakte ho.
Burp ka grep match feature use karo taake woh automatically bata de response mein value nazar aayi ya nahi.

### 🌍 4. Context Samjho

Jahan value reflect ho rahi hai, uska context dekhna zaroori hai:

- HTML body main?

- HTML tag ke andar?

- JavaScript ke andar?

- Attribute ke andar?

Ye context decide karega ke kaunsa payload kaam karega.

# 💥 5. Candidate Payload Test Karo

Ab ek simple payload test karo jaise:

**><script>alert(1)</script>**

Burp Repeater mein request bhejo, response check karo — agar payload as-it-is reflect ho gaya, to alert chal sakta hai.

### Tip: Random value ko payload ke sath rakhna — jese:

**test123<script>alert(1)</script>**

Phir Burp Repeater mein test123 ko search karo, taake tum jaldi se location trace kar sako.

### 🔄 6. Agar Payload Block Ho Gaya To...

Agar website payload ko modify ya block kar rahi hai, to alternate payloads try karo — jaise encoded, broken up payloads, etc.

Jaise:

```><img src=x onerror=alert(1)```

---

Yeh context aur filtering ke hisaab se change hota hai — PortSwigger ka “contexts” section helpful hai ismein.

### 🌐 7. Browser Mein Final Test

Agar Burp Repeater mein payload reflect ho raha hai, to browser mein real test karo — URL ko paste karo aur dekho alert chalta hai ya nahi.

Test payload ke liye:

**alert(document.domain)**

Agar popup aaya, to attack successful hai ✅

---

Yeh dono concepts — **Context** aur **Filtering** — Reflected XSS ko samajhne aur exploit karne ki buniyad hain. Aao inhe aasaan aur practical examples ke sath dekhte hain.

### 1. Context Kya Hota Hai? ("Kahan value reflect ho rahi hai?")
Jab tum kisi input field ya URL parameter mein koi text daalte ho (jaise term=abc), aur server usko page ke HTML code mein wapas print karta hai, toh **wo text HTML code ke kis hisse mein ja kar baith raha hai**, usi jagah ko **Context** kehte hain.
Har context ke apne rules hote hain, is liye har jagah ek jaisa payload nahi chalta. Main contexts yeh hain:
#### a) HTML Body Context (Normal text ke taur par)
Yahan tumhara input do tags ke darmiyan normal text ki tarah aata hai.
 * **Code:** <p>You searched for: abc</p>
 * **Matlab:** Tumhari value <p> aur </p> ke darmiyan aayi hai. Yahan tum <script>alert(1)</script> ya <img src=x onerror=alert(1)> daal kar naya HTML tag bana sakte ho.
#### b) HTML Attribute Context ("HTML tag ke andar" ka matlab)
Jab tumhara input kisi HTML tag ke **attribute** (jaise value="...", href="...", src="...") ke andar jata hai, toh iska matlab hai ke tum pehle se ek open tag ke andar ho.
 * **Code:** <input type="text" name="search" value="abc">
 * **Matlab:** Yahan tumhari abc value <input ...> tag ke **andar** value="..." mein hai. Agar tum direct <script>alert(1)</script> likh doge, toh browser usko attribute ka text samajh lega, execute nahi karega. Pehle double quote " aur tag > close karna parega:
   "><script>alert(1)</script>
   Tab result banega: <input type="text" name="search" value=""><script>alert(1)</script>">
#### c) JavaScript Context
Jab tumhara input pehle se maujood kisi <script> block ke andar jata hai.
 * **Code:** <script>var searchQuery = 'abc';</script>
 * **Matlab:** Yahan tum already JavaScript ke andar ho. Agar tumhe alert chalana hai, toh pehle string 'abc' ko single quote ' se band karna parega, phir apna JS code likhna hoga:
   ';alert(1);//
### 2. Filtering Kya Hoti Hai?
**Filtering** ka matlab hai ke website ka code tumhare input ko page par dikhane se pehle usme se kuch khatarnak characters ya words ko **remove, block, ya encode** kar deta hai taake XSS na ho sake.
 * **Example 1 (Removal/Strip):** Website code likhta hai ke <script> ko delete kar do. Tumne <script>alert(1)</script> bheja, toh website ne alert(1) print kar diya — <script> gayab. Tab tum <img src=x onerror=alert(1)> try karte ho.
 * **Example 2 (HTML Encoding):** Website < ko &lt; aur > ko &gt; mein convert kar deti hai. Is se browser usko tag nahi balkay sirf screen par likha hua text dikhata hai, script run nahi hota.
### 3. Random 8-Character Value (a7x9p3d2) aur Burp Intruder / Grep Match Ka Kya Matlab Hai?
Jab kisi bari website mein **50 ya 100 jagah** input ja sakta ho (URL parameters, forms, headers), toh har jagah manually check karna time leta hai. Is liye yeh trick use hoti hai:
 1. **Random Value Kyun?**
   a7x9p3d2 jaisi random string is liye daalte hain taake ye pehle se page ke HTML mein maujood na ho. Agar yeh response mein dikh jaye, toh 100% yaqeen ho jata hai ke **yehi input reflect ho raha hai**, koi purana text nahi.
 2. **Burp Intruder Ka Kaam:**
   Tum request ko Intruder mein bhejte ho aur mukhtalif parameters ya positions ko target select karte ho taake har jagah test values bheji ja sakein.
 3. **Grep Match Ka Kaam:**
   Burp Intruder ke **Options / Settings** mein **Grep - Match** ka feature hota hai. Tum wahan apni random value (ya reflection indicator) add kar dete ho. Jab Intruder saari requests bhejta hai, toh **Grep - Match** wale column mein tick/highlight aa jata hai ke **kin requests ke response mein woh value wapas aayi hai**. Is se tumhe foran pata chal jata hai ke kaunsa parameter reflect ho raha hai aur kis par aage XSS payload test karna chahiye.

---

Tumne ab poore process ko aapas mein connect kar liya hai, aur tumhari almost saari baatein bilkul theek hain! Bas ek choti si jagah par thori si correction ki zaroorat hai. Aao har baat ka seedha aur clear jawab dekhte hain ke tum kahan theek ho aur kahan thora farq hai:
### 1. Burp Repeater aur Response Tab se khan (ya input) trace karna
 * **Tumhara point:** Data backend/response mein kahan ja raha hai, yeh Repeater ke Response tab se easily dekha ja sakta hai.
 * **Jawab:** **Bilkul theek!** Jab tum khan search kar ke request Repeater mein bhejte ho, toh Response tab mein tum khan search kar ke dekh sakte ho ke woh <p>khan</p> mein hai, value="khan" mein hai, ya <script>...khan...</script> mein.
### 2. HTML Body (<p>), Attribute, aur JavaScript Context
 * **Tumhara point:** <p> ke darmiyan aaye toh aasan payload chal sakta hai; attribute mein ho toh quotes/tag close karna parta hai; JavaScript context mein pehle se <script> ke andar hota hai toh single quote se string close kar ke alert(1) chalate hain.
 * **Jawab:** **Bilkul theek!** Bas yahan ek choti si baat yaad rakhna: JavaScript context mein har baar single quote ' hi nahi hota — kabhi double quote " bhi ho sakta hai, ya bina quotes ke number/variable bhi ho sakta hai (maslan var x = 123;). Wahan code ka structure dekh kar decide karte hain ke kya close karna hai (' ya " ya ;).
### 3. HTML Encoding aur "Iska kya faida?"
 * **Tumhara point:** Agar HTML encoding se < aur > change ho kar script run hi nahi ho rahi, toh phir iska kya faida?
 * **Jawab:** **Yahan thori si baat samajhne ki hai:**
   1. **Attacker ke liye:** Agar full HTML encoding ho rahi hai aur koi bypass nahi mil raha, toh attacker ke liye iska koi faida nahi — matlab **woh input XSS se safe/protected hai** (us jagah vulnerability nahi hai).
   2. **Lekin as a tester/learner iska faida yeh hota hai** ke tumhe yeh samajh aati hai ke *developer ne yahan sahi defense lagaya hua hai* (< ko &lt; encode kiya hai), toh apna waqt zaya karne ke bajaye agle input point par chalo — ya check karo ke kya sirf kuch characters encode ho rahe hain aur koi bypass mumkin hai ya nahi.
### 4. Random 8-Character String (a7x9p3d2) vs Direct Payload
 * **Tumhara point:** Pehle direct payload kyun nahi bhejte? Kyunki context ka farq hota hai aur pehle nahi pata hota ke data kis tag/jagah reflect ho raha hai, is liye pehle random string bhej kar location/reflection dekhte hain, phir context ke mutabiq payload bhejte hain.
 * **Jawab:** **Bilkul theek!** Tumne khud apni baat ka bilkul sahi jawab nikaal liya. Direct har jagah heavy payload bhejney se WAF/filter blocks, noise, aur false impressions aa sakte hain. Pehle clean random string se reflection confirm karna sab se smart aur clean tareeqa hai.
