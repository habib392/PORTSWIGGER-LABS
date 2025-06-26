# Reflected XSS into a JavaScript string with angle brackets HTML encoded

Open website and type in the search box ```'-alert(1)-'```

### Lab Completed

---

### Yeh kis tarah ki technique thi?

- ✅ Type: Reflected XSS
- ✅ Context: JavaScript string injection
- ✅ Technique: Tumne JavaScript string break karke alert(1) inject kiya.

Matlab input kisi JS variable mein gaya, aur tumne ' se string close karke XSS inject kar di.

---

### 🌍 2. Real website par yeh strategy kaise use ho sakti hai?
Jab tum kisi website pe:

Search box, URL parameter, ya koi aur input field use karo...

Aur input ka result page ke JavaScript mein reflect ho raha ho (inspect element se dekh lo):

```<script>```
  ```var search = 'yourInput';```
```</script>```

Toh yeh lab wali strategy apply kar sakte ho:

```'-alert(1)-'```

🎯 Goal: JavaScript string todni hai aur apna code inject karna hai.

---

### 🌟 3. Is lab ki khaas baat kya thi?

Input HTML encode ho raha tha (```< >``` convert ho rahe the), isliye traditional ```<script>``` XSS fail hoti.

But input JavaScript string ke andar reflect ho raha tha.

Tumne HTML tags use kiye bina pure JS string injection se XSS fire karwa diya — smart bypass!

---

### 📆 4. Aaj kal aisi vulnerability milti hai?
🔴 Rare hai — lekin possible hai in cases mein:

Old websites jinhon ne JS string escaping sahi nahi ki.

Custom scripts without frameworks (React, Angular, etc.).

Jab devs sirf ```< >``` encoding karte hain but ' ya " sanitize nahi karte.

✅ Chance: Medium to Low, lekin agar mili toh critical XSS hai.

---

### 🔍 5. Source, Sink, Tag — Detailed Breakdown

- Source	✅ location.search — search query se input jaa raha tha

- Sink	✅ JavaScript string inside a <script> block (```var x = 'input';```)

- Tag	✅ ```<script>``` tag ke andar injection ho raha tha (JS context)
