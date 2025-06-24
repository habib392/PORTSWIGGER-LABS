# (5) DOM XSS in jQuery anchor href attribute sink using location.search source

### 🧠 Main Point:
- Website returnPath naam ke query parameter se value le rahi hai
- Aur usko a tag (anchor/link) ke href attribute mein daal rahi hai
- JavaScript (jQuery) use kar rahi hai
- Aur koi sanitization nahi ho rahi 😈

Yani agar tu chahe to us link ka href="javascript:..." bana ke XSS chala sakta hai

### ✅ Step-by-step Solution:
Step 1:

Website pe jaa aur Submit feedback page open kar.

URL kuch aisa hoga:

```https://0add006703f3b54a82bd0b3500ab0090.web-security-academy.net/feedback?returnPath=/```

Yani ?returnPath= ke baad koi random text daal doo jaisy abc

```https://0add006703f3b54a82bd0b3500ab0090.web-security-academy.net/feedback?returnPath=abc123```

Step 2:
Right-click > Inspect karo ius ky baad ```<div class=container is-page:> ==``` pr click karo phir neechy aik section khuly ga ius main
```<form id``` main jaana hai phir neechy ```<div class``` show hoo jaye ga ius py click kar doo yeh show hoo jaye ga

```<a href="/abc123">Back</a>```

Yani jo value returnPath mein thi wo ab href ke andar aa gayi hai.

Step 3:
Ab tum URL change karo:

```?returnPath=javascript:alert(document.cookie)```

pehly yeh daal kr check karo agr browser ya server ```javascript:``` block kr dy too phir yeh daal do 

```javascript%3Aalert(document.cookie)``` yeh ab chal jaye ga kiun ky yeh encoded version hai

```https://your-lab-id.web-security-academy.net/feedback?returnPath=javascript%3Aalert(document.cookie)```

Hit Enter 🔁

Step 4:
Phir "Back" link pe click karo

Achanak popup aayega jisme:

alert(document.cookie)
✅ XSS Confirmed

LAB SOLVED

