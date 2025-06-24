# Before solving the lab read these important instructions

- location.search matlab URL ka query string (?search=abc)
- document.write() directly woh query page pe daal raha hai.
- Matlab agar hum kuch script inject karein URL mein, woh page pe run ho jata hai.
- Lab ka kehna hai: search query img tag ke andar ja rahi hai. Toh us tag ko break karke hum apna code inject karenge.

  🔹 Step 1: Lab open karo
  
Click on "Access the Lab". Ek search box aur ek search page khulega.

🔹 Step 2: Koi bhi random string search karo

Jaise:
**test123**

🔹 Step 3: Inspect Element karo (Right-click > Inspect)
Dekho tumhara "test123" kaha gaya hai. Tum dekhoge kuch aisa:

```<img src="/image?query=test123">```

---

👉 Matlab search query img ke src attribute ke andar ja rahi hai — yahan se hi injection possible hai.

🔹 Step 4: Injection karo via URL
Ab humein src attribute se bahar nikalna hai, aur apna script inject karna hai.

🔸 Final payload (tum URL mein daalo):

```?search="><svg onload=alert(1)>```

🔹 Full URL ka example:
Agar tumhari lab URL hai:

**https://acde1f123.web-security-academy.net/**

Toh final payload wala URL hoga:

```https://acde1f123.web-security-academy.net/?search="><svg onload=alert(1)>```

🔹 Step 5: Open that URL

Jaise hi tum ye URL open karoge, alert(1) pop-up ho jayega.

🎉 Lab complete ho gaya!

🔐 Penetration Testing Tip:

- DOM XSS zyada dangerous hoti hai kyunke server-side filtering nahi hoti.
- document.write(location.search) jaise sinks hamesha risky hote hain.
- Agar tum client-side JS dekh kar aise sinks locate kar lo, to tum bypass create kar saktay ho even jab server filter kar raha ho.

