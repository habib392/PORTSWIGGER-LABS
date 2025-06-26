# DOM XSS in jQuery selector sink using a hashchange event

### 🧠 Pehle lab ko samajh:

⚙️ Kya ho raha hai?

Website location ka hash part use kar rahi hai (i.e. URL ke #something ke baad wala)

Wo hash jQuery ke $() selector mein directly use ho raha hai

Jab hash change hota hai, page kisi element pe scroll ya action perform karta hai

Agar attacker ne koi HTML/JS tag hash mein daal diya aur wo directly $() mein chala gaya → XSS ho sakta hai

🧨 Vulnerable JavaScript code kuch aisa hota hai:

```$(window).on('hashchange', function() {
  var post = location.hash;
$(post).addClass("highlight"); // ← Yeh sink hai
});```

Yahan location.hash directly jQuery ke $() mein gaya — yeh DOM XSS ka classic sink hai 💣

### 🎯 Lab Goal:
Victim ke browser mein print() function execute karwana hai.

### ✅ Solution Step-by-Step:

🔹 Step 1: Exploit Server open karo
Lab page pe upar "Go to exploit server" likha hoga

Us pe click karo

🔹 Step 2: Malicious payload iframe mein daalo:

```<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>```

🔍 Note:

Yeh iframe victim ke browser mein website open karta hai

onload event chalta hai

Wo src mein XSS payload append karta hai → ```#<img src=x onerror=print()>```

Wo hash DOM mein jQuery sink mein chala jata hai

```onerror=print()``` execute ho jata hai ✅

🔹 Step 3: Click “Store exploit”
🔹 Step 4: Click “View exploit”

Dekho kya print dialog open hota hai

Agar haan → payload successful hai ✅

🔹 Step 5: Click “Deliver to victim”
Lab solve ho jaye ga!

### Summary:
Yeh XSS location.hash ki wajah se ho rahi hai jo directly jQuery ke $() mein chali gayi bina filter ke.
Tu iframe ka use kar ke hash change karta hai aur payload execute hota hai — print() browser ka native function hai, alert ki jagah usi ko trigger karna tha.
