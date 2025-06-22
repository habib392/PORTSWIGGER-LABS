# 🧠 DOM-Based XSS

### 🔍 DOM XSS Kya Hota Hai?

DOM ka matlab hota hai: Document Object Model, yani jo cheezein browser load karta hai — HTML, JavaScript, etc.

DOM-Based XSS tab hota hai jab:

1. JavaScript kisi attacker ke control wali jagah (source) se data uthata hai

2. Aur us data ko aisi jagah (sink) mein dalta hai jo dangerous ho — jaisy innerHTML, eval(), etc.

Matlab: Attacker agar URL mein malicious JavaScript daal de aur page usay process kar le bina sanitize kiye, to attack ho jata hai.

## 🧪 Source Aur Sink Kya Hote Hain?

### ✅ Common Sources (jahan se data uthta hai):

- window.location

- location.search → query string

- location.hash → URL ke baad #value

- document.URL

- document.referrer


### ❌ Common Sinks (jahan pe attacker ka payload inject hota hai):

- innerHTML

- document.write()

- eval()

- setTimeout() (agar string input ho)

- location.assign()

### 🧨 DOM XSS Kaise Hota Hai?

Suppose yeh JavaScript code hai:

**let userInput = location.hash;
document.getElementById("output").innerHTML = userInput;**

Ab agar koi user yeh URL visit kare:

**#https://example.com/page.html#<script>alert(1)</script>**

To page us payload ko directly innerHTML mein daal dega → alert box dikhega → DOM XSS ho gaya.

### 🔬 DOM XSS Test Karne Ka Tareeqa

1. Browser (like Chrome) khol lo.

2. URL ke kisi part (search, hash) mein ek random string daalo:
**#xss1234**

3. Developer Tools (Inspect Element) khol ke DOM ko search karo:

Ctrl + F daba kar search karo: **xss1234**

4. Dekho wo string DOM mein kahan reflect hui:

**div, attribute, script, etc.**

5. Ab us jagah ke hisaab se payload banao:

Agar double-quoted attribute mein gaya:
xss" onmouseover="alert(1) try karo.

### 💡 Browser Encoding Farq Karta Hai

Chrome, Firefox, Safari: location.hash aur location.search ko encode karte hain

IE11 aur purane Edge nahi karte

To agar JavaScript decode na kare to XSS kaam nahi karega.
