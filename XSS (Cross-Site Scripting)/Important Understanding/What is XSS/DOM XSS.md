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

### 🧪 DOM XSS Testing with DOM Invader

Normally jab hum DOM-based XSS dhoondhtay hain, to humein JavaScript ka complex code manually check karna padta hai – jo boring aur time-consuming hota hai, especially jab code minified ho (chhupa hua aur compressed).

Lekin agar tu Burp Suite ka built-in browser use karta hai (jo Chromium-based hai), to usme ek zabardast tool hota hai jiska naam hai DOM Invader. Yeh extension automatically:

DOM XSS ke sources aur sinks ko detect karta hai

Jo input tu daalta hai, uska flow trace karta hai ke wo kaunsa JavaScript function (sink) tak ja raha hai

Agar koi dangerous jagah par reflection milti hai, to wo tujhe warning deta hai

Matlab tu time waste kiye baghair seedha payload test kar sakta hai aur vulnerable jagah pehle hi highlight ho jati hai.

### 💡 Real-World Example:

Tu agar search bar mein kuch JavaScript daalta hai, DOM Invader tujhe show karega ke wo innerHTML mein gaya ya eval() mein. Agar vulnerable ho to tu turant alert(1) ya koi bhi payload try karke attack confirm kar sakta hai.

### 🛠️ Lab karne ke waqt DOM Invader ka faida:

Tujhe manually DOM trace nahi karna padta

Jaldi pata chal jata hai ke reflection ho rahi hai ya nahi

Tere payload ka effect kya ho raha hai, wo browser mein easily dikhta hai