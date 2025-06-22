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