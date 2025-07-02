# Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped

## 🔧 Step-by-Step Solution (Burp Suite ke sath):

### Step 1: Random input do aur check karo
Website open karo.

Search box mein kuch random daalo jaise abc123.

Burp Suite on karo aur Intercept chalu rakho.

Ab request capture hogi. Us request ko "Send to Repeater" karo.

---

### Step 2: Reflected input check karo
Burp Repeater mein request ko Send karo.

Response tab mein dekho:

```var search = 'abc123';```

Yani input JavaScript string ke andar aa rahi hai.

---

### Step 3: Encoding behavior test karo
Ab input change karo:

```test'```
Request send karo aur dekho:

```var search = 'test\'payload';```

Dekha? Single quote escape ho gaya hai ```\'.```

Ab test karo:

```test\```

Dekho response mein kya aata hai:

```var search = 'test\payload';```

Matlab backslash escape nahi ho raha, yeh important point hai!

---

### Step 4: Final payload try karo
Yeh payload daalo search box mein ya Repeater mein input mein:

```\'-alert(1)//```

Explanation:

- \' string close kiya

```-``` safe character

```alert(1)``` XSS payload

```//``` baki JS code comment out kar diya

Ab Copy URL karo, browser mein paste karo aur open karo.

---

### ✅ Step 5: Alert(1) ka pop-up aaye to lab complete!

---

### 💡 Real Life PenTest Tip:
Jab tum JS context mein XSS test kar rahe ho aur single quote escape ho raha ho,
to backslash injection ke sath try karo jaise is lab mein hua. Yeh modern bypass technique
hai jo real websites mein bhi kaam kar sakti hai, especially jahan escaping partial ho.

---

### Aik Important Baat 
Jab hum request ko burpsuite sy repeater main bhejyein gyein phir send pr click karien gyien too aik cheez dekho gy ap 
ky jo apny input daala hai jaisy test\ yeh green hoo rha hai string main isko black karna hai tab hi yeh execute hoga 
koi bhi input daalo wo green hoo jaye gi lekin jab hum iusko exploit karein gyein jaisy iss ```\'-alert(1)//``` command sy
too alert(1) joo hai wo black hoo jaye ga or successfully execute hoo jaye ga
