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