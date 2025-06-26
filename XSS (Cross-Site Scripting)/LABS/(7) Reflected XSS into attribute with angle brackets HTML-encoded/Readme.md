# Reflected XSS into attribute with angle brackets HTML-encoded

🔍 Lab Ka Goal:

Tujhe XSS fire karna hai HTML attribute ke andar, lekin < > (angle brackets) encode ho ja rahe hain.
Yani tu <img> tag ya <script> directly nahi daal sakta.

### ✅ Kaise Samjhein?
Tu agar search kare **test123**

View source karo ya Burp mein response dekho — tu yeh dekh payega:

```<input value="test123">```

Yani tera input gaya input tag ke value attribute ke andar

Aur "quotes ke andar gaya hai ("test123")

Ab agar tu angle brackets (<, >) daalta hai to HTML encode ho jaata hai:

< → &lt;

> → &gt;

Toh HTML tag break karna kaam nahi karega

### ✅ Ab kya karna hai?

Tujhe quote break karna hai aur event handler inject karna hai.

Working payload:

```"onmouseover="alert(1)```

Jab tu yeh payload input mein daalega:

```<input value=""onmouseover="alert(1)">```

Toh browser jaise hi mouse uss element pe le jaayega → XSS fire ho jayega 🎯

### LAB SOLVED

