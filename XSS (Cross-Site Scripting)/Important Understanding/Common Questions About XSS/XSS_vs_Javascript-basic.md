# JavaScript aur XSS – Asaan Lafzon Mein Samjho

---

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