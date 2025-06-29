# JavaScript aur XSS – Asaan Lafzon Mein Samjho

## 🔹 JavaScript Kya Hai?

JavaScript browser ki zuban hoti hai jo webpages ko zinda banati hai.  
Jaise:
- Button dabao to kuch ho
- Form validate ho
- Alert box aaye
- Page reload ke bina data aaye (AJAX)

---

## 🔹 XSS Kya Hota Hai?

XSS ka matlab hai **Cross Site Scripting**  
Yeh tab hota hai jab attacker apna **JavaScript ka code** kisi website ke andar daal de,  
aur wo code kisi aur user ke browser mein **chal jaye**.

### 🧠 XSS se attacker kya kar sakta hai?
- Cookies chura sakta hai 🍪  
- Login session hijack kar sakta hai 🔐  
- User ko phishing page pe bhej sakta hai 🎣  
- Admin ko apni script bhej kar kuch bhi karwa sakta hai

---

## 🔗 JavaScript aur XSS ka Rishta

Agar website JavaScript use hi nahi karti,  
to XSS ka **koi faida nahi** hota.  
XSS tabhi kaam karta hai jab browser JavaScript **run karta hai**.

---

## 🔥 XSS Payloads – Jo attacker inject karta hai

```html```
```<script>alert(1)</script>```
```<img src=x onerror=alert(1)>```
```<svg= x onload=alert(1)>```

Yeh sab tricks hain taake browser attacker ka code chala de.

---

### 🔄 Source aur Sink ka Scene

Source: Jahaan se user ka input aata hai
Jaise: location.search, location.hash, document.URL

Sink: Jahaan pe input ko JS use karti hai
Jaise: innerHTML, eval(), document.write()

Agar input directly source se sink mein chala gaya bina filter ke,
to samajh lo XSS ready hai.

🎯 Example:

```let input = location.search;
document.getElementById('output').innerHTML = input;```

Aur agar attacker URL mein yeh daal de:

```?input=<script>alert(1)</script>```

Toh browser alert chala dega — XSS done!

❗ Bohat Important Point

Attacker ka JavaScript payload website ke apne JavaScript code mein hi ghusa diya jata hai.

Browser usse pehchan nahi pata — usse lagta hai yeh bhi normal JS hai —
aur wo use chala deta hai.

---

✅ Aakhri Baat – Sab kuch ek line mein:

"Attacker apna JavaScript inject karta hai, browser usse website ka hissa samajh ke chala deta hai — isey kehte hain XSS."

---

### 🔐 Tips (Penetration Testing Style)

Inspect element (F12) karke dekho JS kahan input le rahi hai

Dekho kya innerHTML, eval(), ya document.write() use ho raha hai

DOM-Based XSS mostly client-side hota hai — server ko pata bhi nahi chalta