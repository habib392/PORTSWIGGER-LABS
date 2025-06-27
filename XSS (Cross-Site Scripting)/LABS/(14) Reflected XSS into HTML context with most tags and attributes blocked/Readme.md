# Reflected XSS into HTML context with most tags and attributes blocked

🎯 Goal:

Print function print() ko automatically trigger karna via reflected XSS — bina user interaction ke (no click, no manual input).


---

🧠 Key Concepts:

App ek WAF use kar raha hai jo <script>, <img>, onerror, etc. jaise common payloads ko block karta hai.

Tu Burp Suite + smart filtering bypass se check karega kaunsa tag aur attribute allowed hai.

<body onresize=print()> allowed hai, so we'll use that.



---

✅ Tools Required:

Burp Suite (Community or Pro)

Burp Browser (built-in browser)



---

🚶‍♂️ Step-by-Step Solution:


---

🔹 Step 1: Open Lab in Burp Browser

Burp Suite open karo → Burp ka built-in browser launch karo → Lab URL open karo


---

🔹 Step 2: Trigger a Search Request

Lab website par koi random word search karo (jaise: test)
→ Request HTTP History mein show hogi.


---

🔹 Step 3: Send Request to Burp Intruder

Request par right click karo → Send to Intruder

Intruder tab mein jao



---

🔹 Step 4: Find Reflected Tag

Search parameter mein <> daalo

Cursor ko beech mein le jao <§§>

Payload position add karo (click “Add §”)

Ab tu different HTML tags test karega



---

🔹 Step 5: Load All HTML Tags

Intruder ke Payloads tab mein jao

Payloads config mein “Paste” pe click karo

PortSwigger XSS Cheat Sheet se Copy all tags karke paste karo

Start Attack karo


🔍 Check karo:

Most tags → 400 error

<body> tag → 200 OK ✅



---

🔹 Step 6: Test Body Tag Attributes

Now use this:

<body%20=1>

Cursor le jao = se pehle → <body%20§§=1>

New payload position set karo



---

🔹 Step 7: Load Event Attributes

Intruder Payloads tab mein jao

Purane payloads Clear karo

XSS Cheat Sheet se Copy Events (like onload, onresize, etc.)

Paste payloads → Start Attack


🔍 Check karo:

Most attributes → 400 error

onresize → 200 OK ✅



---

🔹 Step 8: Final Payload Banaao

Lab ne search param reflect kiya hai, so tu payload manually encode karega:

"> <body onresize=print()>

URL encode ho jaayega:

%22%3E%3Cbody%20onresize%3Dprint()%3E


---

🔹 Step 9: Use Exploit Server

Exploit server open karo → HTML mein yeh code paste karo:

<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize%3Dprint()%3E" onload="this.style.width='100px'">

✅ Lab ID wala part apne lab ke mutabiq replace karo

Click “Store”

Click “Deliver to victim”


Agar sab sahi hua to victim jab iframe load karega, body onresize execute ho jaayega → print() call → ✅ Lab Solved


---

📌 Final Payload (Decoded):

"><body onresize=print()>

→ Triggered automatically when body resizes via iframe load


---

🧠 What We Learned:

Point	Detail

Type	Reflected XSS
Context	HTML
Challenge	WAF blocking tags and attributes
Bypass	Use of <body onresize=print()>
Trigger	Automatically via iframe resize
No user interaction	✅ Required per lab instructions