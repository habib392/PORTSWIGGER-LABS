# Before solving the lab read these important instructions

- location.search matlab URL ka query string (?search=abc)
- document.write() directly woh query page pe daal raha hai.
- Matlab agar hum kuch script inject karein URL mein, woh page pe run ho jata hai.
- Lab ka kehna hai: search query img tag ke andar ja rahi hai. Toh us tag ko break karke hum apna code inject karenge.

# Now Solve the Lab
  🔹 Step 1: Lab open karo
  
Click on "Access the Lab". Ek search box aur ek search page khulega.

🔹 Step 2: Koi bhi random string search karo

Jaise:
**test123**

agr url main aisy show hoo 

**https://0adf00be04c3d95c80913fcf00e100a5.web-security-academy.net/?search=test123**

too samj jao website vulnerable hai phir url main yeh daalo

```https://0adf00be04c3d95c80913fcf00e100a5.web-security-academy.net/?search="><svg onload=alert(1)>```

🔹 Step 3: Open that URL

Jaise hi tum ye URL open karoge, alert(1) pop-up ho jayega.

## Lab Complete

🔐 Penetration Testing Tip:

DOM-based XSS detect karne ke liye:

Search box test karo

view-source: ya Ctrl+U se JS code dekho

Agar tumhein:

- document.write()

- innerHTML

- eval()

- setTimeout(..., 0)

- location.search / hash / referrer

milay aur input controlled ho ⇒ target pakka hai Matlab website main DOM possible hai

---

- DOM XSS zyada dangerous hoti hai kyunke server-side filtering nahi hoti.
- document.write(location.search) jaise sinks hamesha risky hote hain.
- Agar tum client-side JS dekh kar aise sinks locate kar lo, to tum bypass create kar saktay ho even jab server filter kar raha ho.

