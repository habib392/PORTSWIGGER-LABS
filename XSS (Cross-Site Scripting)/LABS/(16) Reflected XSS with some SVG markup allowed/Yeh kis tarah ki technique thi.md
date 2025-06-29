# Questions/Answers

### 🔥 1. Yeh kis tarah ki technique thi?
Yeh Reflected XSS (Bypass via SVG) technique thi.

Website ne common tags jaise ```<script>``` ya ```<img onerror>``` block kar diye thay, lekin uncommon SVG tags jaise ```<svg>``` aur event onbegin ko block nahi kiya. Tumne yeh bypass nikaala using Burp Intruder aur payload testing.

---

🧪 2. Real website mein yeh strategy kaise use hogi?

Jab tum koi real site test kar rahe ho:

🔍 Step-by-step Strategy:

User input point find karo: URL params, forms, search boxes, etc.

Payload inject karo:

Start simple: ```<script>alert(1)</script>```

Agar block ho jaye → try SVG-based payloads

Response inspect karo (Inspect Element ya Burp Response):

- Kya payload reflected hai?
- Kya encode ho gaya?
- Kya tag cut gaya?

Agar encode nahi hua, lekin alert nahi chala, toh event-based XSS try karo, jaise onload, onmouseover, onbegin inside allowed tags like ```<svg>```

