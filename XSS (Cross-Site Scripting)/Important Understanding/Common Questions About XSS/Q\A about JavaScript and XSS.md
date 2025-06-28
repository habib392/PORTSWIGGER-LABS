# Questions Answers About XSS and JavaScript 

✅ Q1: Agar attacker ```<img>``` ya <iframe> jese tag URL mein daalein to XSS possible hai?

🔍 Jawaab:

Haan! 100% correct.
Agar URL ke through attacker ka input jaa raha hai kisi HTML element ke andar (like ```<a>, <img>, <iframe>,``` etc.)
Aur wo input sanitize nahi ho raha, to attacker:

attribute break kar ke

malicious HTML inject kar sakta hai

aur JavaScript payload (like alert(1)) execute karwa sakta hai

🎯 Example:

```<a href="/feedback?returnPath=javascript:alert(1)">```

Ya phir:

```<img src=x onerror=alert(1)>```

Toh jab wo input href, src, innerHTML, document.write mein chala jaye bina filtering ke → XSS possible hai

---

### ✅ Real Websites pe kaise pata chalega kaunsa tag allow hai?

1. Tum input ya URL parameter mein HTML inject karo:

```<b>test</b>``` 
```<script>alert(1)</script>```
```<xyz onmouseover=alert(1)>abc</xyz>```


2. Fir dekho output mein kaunsa tag clean ho gaya aur kaunsa raw print ho gaya.


3. Agar ```<b>``` visible hai, to woh allowed hai. Agar ```<script>``` remove ho gaya ya encode ho gaya ```(&lt;script&gt;)``` to woh blocked hai.


✔️ Tip:

Burp Suite se response body dekho — wahaan pata chalega frontend ne filter kiya ya backend ne.