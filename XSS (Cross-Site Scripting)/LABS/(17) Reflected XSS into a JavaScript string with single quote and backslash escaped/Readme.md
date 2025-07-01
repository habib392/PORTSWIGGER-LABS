## Lab 17: Reflected XSS into a JavaScript string with single quote and backslash escaped

---

### 🔍 Step-by-Step A to Z:

### ✅ Step 1: Random Search Input

Website khol kar search box main koi bhi random word likho jaise test123 aur Enter karo.

Burp Suite ON rakho aur request ko intercept karo.

Phir us request ko Burp Repeater main bhej do (right click → send to repeater).

---

### ✅ Step 2: Reflection Samajhna

Repeater tab main search karo test123 ko kahaan reflect ho raha hai.

Tum dekhogay kuch aisa:

```<script>
  var searchTerm = 'test123';
</script>```

Yahan pe tumhara input 'single quotes' ke andar hai → JavaScript string ke andar.

---

### ❌ Step 3: Normal Payload Fail Hoga

Ab agar tum yeh payload bhej do:

test'alert(1)

To response main yeh dikhega:

var searchTerm = ```'test\'alert(1)';```

Dekha? ' quote ko \ escape kar raha hai. Toh string break nahin hogi, aur script execute nahin hogi.

---

### ✅ Step 4: Bypass Strategy — Full Injection

Ab humein JavaScript ke string se nikalna hai aur apna payload inject karna hai. Yeh kaam karega:

```</script><script>alert(1)</script>```

🧠 Logic:

Pehle </script> lagakar existing <script> tag ko band kar do.

Phir apna ```<script>alert(1)</script>``` lagao — yeh naya script tag execute hoga.

Yeh reflected hoga kuch aise:

```<script>
  var searchTerm = '</script><script>alert(1)</script>';
</script>```

Browser isko parse karega aur alert(1) trigger karega.

---

### ✅ Step 5: Test Payload in Browser

Repeater se working payload request ko "Copy URL" karo.

Paste karo browser main aur load karo.

Agar alert box pop ho gaya → 🎉 Lab Solved!

---

### 🧪 Final Working Payload:

```</script><script>alert(1)</script>```

---

### 💡 PenTest Tip (Jaise tu chahta hai):

Yeh technique un websites pe kaam aati hai jo:

JavaScript ke andar user input rakh rahi hoti hain,

aur input ko ' aur \ se escape kar rahi hoti hain,

Lekin HTML escaping nahi kar rahi hoti.

---

### Tum isko penetration testing main test karne ke liye:

- View source ya response analyze karo,

- Check karo input kis context main reflect ho raha hai,

- Aur phir usi context ka XSS bypass use karo.