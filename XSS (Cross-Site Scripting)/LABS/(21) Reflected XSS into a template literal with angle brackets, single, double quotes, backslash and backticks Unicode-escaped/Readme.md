# Lab Title
**Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped**

---

### 🧠 Lab Ka Core Concept:

Website tumhara input JavaScript template string (let data = ${input}``) ke andar reflect kar rahi hai.

But:

< >, ' ", \, ` sab escape kiye ja rahe hain

Only ```${}``` allowed kyunki wo template literal ka native part hai

### 👉 Iska matlab: Tum normal tag ya quote wala XSS payload nahi daal sakte

Sirf ek valid way hai:

```${alert(1)}```
Yeh payload template string ke andar JS execute karwa deta hai ✅

---

### ✅ Step-by-Step Lab Solution (BurpSuite + Browser)

---

#### 🔹 Step 1: Lab Open Karo

Lab launch karo

Search box pe jao (jahan blog search hoti hai)

---

#### 🔹 Step 2: Random Search Karo

Example: test123 likho

Burp Suite ka Proxy ON rakho

Search karo aur Burp mein intercept aayega

---

#### 🔹 Step 3: Burp Repeater mein Bhejo

Request intercept hui to Right click → Send to Repeater

Proxy forward kar do ya disable kar do

---

#### 🔹 Step 4: Response mein Reflection Check Karo

Repeater tab khol kar Send karo

Response dekho, tumhara test123 kis tarah reflect hua hai

Example:

```let data = `${test123}`;```

👉 Yeh confirm karega ke input template string ke andar ja raha hai
Aur quotes/angle brackets/backticks escape ho chuke hain

---

#### 🔹 Step 5: Payload Daalo

Ab test123 ki jagah payload daalo:

```${alert(1)}```

Toh full line kuch aisi banegi:

let data = ````${${alert(1)}}`;```

But browser isko render karega:

let data = ````${alert(1)}`;```

Jo valid JS execution hai 🔥

---

#### 🔹 Step 6: Response Test Karo (Browser mein)

Burp Repeater se Right click → Copy URL

Chrome ya Firefox browser mein paste karo

Enter press karo

🚨 Alert box popup ho jayega = XSS successfully executed 🎉

---

##### 🛡️ Cyber Security Angle (PenTest View):

Template literals (${}) JS mein dynamic value inject karte hain

Agar koi app bina sanitize kiye user input ko template string mein inject kare = XSS possible

Real life mein yeh mostly hota hai JS-based apps (React, Angular, etc.) mein