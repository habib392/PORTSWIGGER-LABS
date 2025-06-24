# 💣 Yeh kaunsi technique thi?

### Technique ka naam:
DOM-Based Cross-Site Scripting (DOM XSS)

Is example mein:

- Source: location.search
- Sink: innerHTML
- Result: Malicious HTML code inject ho gaya

Iska matlab:

Tumhara payload browser ke andar JavaScript se page ke kisi element mein daal diya gaya — bina server touch kiye!

### 📌 DOM XSS use kab hota hai Penetration Testing mein?

✅ Jab:
- User input page URL mein jata hai (jaise ?search=test)
- JavaScript us input ko process karta hai
- Aur woh input kisi dangerous sink mein jata hai (like innerHTML, eval, document.write)
- Sanitize na kiya gaya ho

Tab DOM XSS ka chance hota hai!

### 🎯 Real-World Example:
Agar tum kisi search page, feedback form, ya filter system pe ho:

Tum URL mein kuch payload inject karte ho:
```?search=<img src=x onerror=alert(1)>```

Aur agar page reload pe popup aata hai → Bingo! XSS mil gaya

### 💥 Real World Exploitation:
Agar DOM XSS mil jaaye to:

- ✅ Steal cookies
- ✅ Redirect user
- ✅ Show fake login page
- ✅ Load malicious JS files
- ✅ Bypass some CSP or WAFs (kyunke server ko touch nahi kiya)

### 🔐 Important Note:
DOM XSS mostly client-side hota hai
Yani server logs mein bhi kuch na aaye — that's why dangerous aur chhupkar hota hai 😈

📘 Summary:

Technique: DOM XSS

Kab use karte ho: Jab user input location.search, hash, ya referrer se JS mein jaye

Sink: Agar innerHTML, eval, document.write jese dangerous sink mein jaye

Use case: Web app ka front-end break karna / exploit karna without touching server

