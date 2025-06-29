# Questions/Answers

### 🔥 1. Yeh kis tarah ki technique thi?
Yeh Reflected XSS (Bypass via SVG) technique thi.

Website ne common tags jaise ```<script>``` ya ```<img onerror>``` block kar diye thay, lekin uncommon SVG tags jaise ```<svg>``` aur event onbegin ko block nahi kiya. Tumne yeh bypass nikaala using Burp Intruder aur payload testing.

---

### 🧪 2. Real website mein yeh strategy kaise use hogi?

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

---

### Lab mein kya khaas baat thi?

| Feature                       | Detail                                    |
| ----------------------------- | ----------------------------------------- |
| 🛡️ **Filter laga tha**       | Common tags/attributes block ho rahe thay |
| 🎨 **SVG allow tha**          | `<svg>`, `<animatetransform>` allowed     |
| ⚙️ **Uncommon event allowed** | `onbegin`                                 |
| 🧠 **Bypass required logic**  | Brute-force via Burp Intruder             |
| 🔍 **Reflection visible**     | Input was reflected directly in the DOM   |

---

### 📅 4. Aaj kal milti hai aisi vulnerability?

✅ Mil sakti hai, lekin rare hai:
Modern apps mein filters zyada strong hote hain

Lekin agar developer ne custom filter banaya ho, chances badh jaate hain bypass hone ke

Real-life mein yeh vulnerability milegi:
Purani ya poorly coded websites mein

WordPress ya CMS plugins mein

Custom JavaScript render apps mein (jaise React ya Angular agar insecure code ho)

---

### 🧩 5. Source kya tha?
👉 location.search

Tumhara payload ?search=... ke through aa raha tha, aur page ne directly usko HTML mein inject kar diya.

---

### 🧠 6. Sink kya tha?
👉 Most probably innerHTML ya document.write

Kyuki payload HTML ke form mein page pe reflect ho raha tha bina encoding ke.

---

### 📦 7. Kaunsa tag tha jisme injection ho raha tha?
👉 Injection kisi container tag ke andar ho raha tha — like:

```<div>Search results for``` ```<b>YOUR_INPUT_HERE</b></div>```

Tumhara payload ```<svg><animatetransform onbegin=alert(1)>``` us div ke andar gaya.