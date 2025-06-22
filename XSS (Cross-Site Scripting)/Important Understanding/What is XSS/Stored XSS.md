# 💾 Stored XSS 

Stored XSS ka matlab hota hai:

Website attacker se koi input leti hai, usko save kar leti hai (jaise database mein)
aur baad mein jab dusra user woh page dekhta hai, to attacker ka JavaScript code uske browser mein chal jata hai.

### 🧪 Example:

Ek message board app hai, jahan log apna message likh kar bhejte hain. Jaise:

**<p>Hello, this is my message!</p>**

Lekin app is message pe koi filtering nahi karti, to attacker aisa message bhej sakta hai:

**<p><script>alert('XSS!')</script></p>**

Ab jab koi aur user ye message dekhega, uske browser mein attacker ka JavaScript chal jayega.


### 📥 Kahaan kahaan se input aa sakta hai?

Blog comment, Chat nickname, User profile info, Webmail ka email content, Social media post, Network logs

Ye sab agar filter na hon, to attacker inke zariye JavaScript inject kar sakta hai.

### 📌 Stored XSS ko doosra naam bhi milta hai:

- Persistent XSS
- Second-order XSS

# 💥 Stored XSS ka Asar

###🧠 Sabse pehle:

Agar attacker ka JavaScript kisi victim ke browser mein chal jaaye, to wo user pooray tareeqe se hack ho sakta hai.

Attacker:

User ki cookies le sakta hai

User ke naam pe action kar sakta hai (form submit, transfer, etc.)

Dusre users ko bhi target kar sakta hai

Basically: jaisa user kar sakta hai, attacker bhi kar sakta hai

###🧨 Stored XSS ka khaas nuksaan ye hai:

Attacker ko kuch bhi send nahi karna padta, sirf application mein payload daal deta hai —
jab koi bhi user uss page pe jata hai (aur login hota hai), JavaScript auto chal jaata hai

###💬 Real life example:

Attacker ne feedback form mein **<script>...</script>** daal diya

Website ne woh feedback store kar liya

Jab admin panel ya user panel khulta hai, attacker ka code chal jaata hai

Victim ko kuch samajhne ka waqt hi nahi milta — exploit auto fire ho jaata hai 💥

# 🧠 Stored XSS in Different Contexts

Dekho bhai, stored XSS sirf ek hi style mein nahi hoti. Yeh depend karta hai ke:

📍1. Stored Data Website ke Response mein Kahan Appear ho rahi hai?

Jaise:

HTML ke andar <p>Yahan</p>

Attribute ke andar <img src="Yahan">

JavaScript ke andar var x = "Yahan";

Toh payload bhi usi hisaab se banana padta hai. Har jagah same script nahi chalti.

### 🧪 Example:

Agar data HTML tag ke andar inject ho raha hai:

**<p><script>alert(1)</script></p>**

Agar data kisi attribute mein hai:

**<img src="x" onerror="alert(1)">**

Agar JavaScript block mein hai:

**var msg = "<script>alert(1)</script>";**


🔍 2. Website agar koi filtering ya validation karti hai...

Toh fir attacker ko thoda smart hona padta hai. Payload aisa banana hota hai jo:

Validation se bach jaye

Modify hone ke baad bhi execute ho jaye


Jaise: Agar **<script>** block ho jaye, toh attacker img onerror ya iframe srcdoc ya encoded payloads ka use karega.

# 🛣️ Entry Points Dhoondhna

Entry point matlab wo jagah jahan attacker data inject kar sakta hai. Jaise:

- 1. URL parameters:
Example: **?name=Habib<script>**

- 2. Message body (POST request):
Example: feedback forms, comment sections.

- 3. URL path:
Example: **/profile/habib<script>**

- 4. HTTP headers:
Rarely used in reflected XSS, lekin stored XSS mein kaam aa sakte hain (jaise User-Agent).


- 5. Out-of-band sources:

Email input in webmail apps

Tweets in Twitter feed viewers

News snippets from RSS feeds
=> Yeh sab bhi attacker-controlled input ho sakta hai agar app us data ko show karti ho.

### 📤 Exit Points Dhoondhna

Exit point matlab wo jagah jahan pehle daala gaya data dikhaya jaata hai — chahe kisi bhi user ko.

- Comments display ho rahe hain

- Admin panel mein logs aa rahe hain

- Chat messages ya notifications aa rahi hain

- Even obscure audit logs ya profile previews

👉 Kahin bhi data wapas aa raha hai, wo exit point ho sakta hai.

### 😓 Problem Kya Hoti Hai?

1. Link bananay mein dikkat:
Data kis entry se gaya aur kis exit pe dikha — isko map karna mushkil hota hai, especially jab pages zyada hon.

2. Data overwrite ho jata hai:
Jaise agar search history save hoti hai to wo naye searches se replace ho jati hai. To pehle daala hua payload milna mushkil ho jata hai.

⚙️ Smart Manual Approach

Full brute-force na karo. Systematic approach follow karo:

1. Har entry point mein ek unique value daalo, jaise **xss1234**

2. Phir app mein different pages pe jaa kar dekho ye value kahin dikh rahi hai ya nahi

3. Jab value mil jaye, samjho ye stored hai (agar immediately reflect na hui ho)

4. Ab dekho kis context mein hai — HTML, JS block, attribute, etc.


5. Ab uss context ke hisaab se XSS payload test karo, jaise:

**<script>alert(document.cookie)</script>**


### 🔄 Testing Flow Summary:

Step	Action

1️⃣	Entry point find karo
2️⃣	Unique value inject karo
3️⃣	Exit point locate karo
4️⃣	Context analyze karo
5️⃣	Payload inject karo
6️⃣	Browser mein test karo