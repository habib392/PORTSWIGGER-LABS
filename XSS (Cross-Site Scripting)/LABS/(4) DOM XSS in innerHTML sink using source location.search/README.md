# DOM XSS in innerHTML sink using source location.search

### 🧠 Key baat kya hai?

✅ Tumhara input location.search se uth raha hai (URL ka ?search= wala part)

✅ Aur uss input ko directly element.innerHTML = input mein daal diya gaya hai

✅ Agar sanitize nahi kiya gaya = tumhara HTML code as it is render hoga → XSS ka chance

### 🔍 Step-by-step Solution:
✅ Step 1:

Search box mein ye likho:

```<img src=1 onerror=alert(1)>```

Yeh kya karega?:

```<img src=1>``` — galat image hai (kyunke “1” image nahi)

onerror=alert(1) — jab image load na ho, alert(1) chal jaye

✅ Step 2:

Click on Search button.

Ab kya hoga?:

Tumhara input URL mein chala gaya:
```?search=<img src=1 onerror=alert(1)>```

JavaScript ne location.search se input uthaya

Fir woh input directly innerHTML mein chala gaya
Like:

```document.getElementById("search-results").innerHTML = userInput;```

Matlab: ```<img src=1 onerror=alert(1)>``` actual HTML ban gaya → browser render karega → XSS ho gaya ✅

### 🔐 Iska Use Penetration Testing mein kya hai?
Tum check karte ho:

Kya input location.search se liya gaya hai?

Kya woh input innerHTML mein gaya hai?

Agar haan → payload inject karo → agar pop-up aata hai → DOM XSS confirmed

### 🧪 Bonus Tip:

Agar <img> payload work na kare kisi site pe, to ye test karna:

```<script>alert(1)</script>```

Lekin innerHTML mein mostly <img> ya <svg> zyada safe bet hotay hain.
