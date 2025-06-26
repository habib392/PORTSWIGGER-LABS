# Yeh technique kis type ki XSS hai
🧠 Yeh kaunsi technique thi?
✅ Reflected XSS — Attribute Injection based
Reflected XSS: Jo input tu URL ke through bhejta hai, woh immediately response mein aa jaata hai bina proper sanitization ke.

Yeh attack HTML attribute ke andar hua, jahan tera input quotes " " ke andar gaya:

```<input value="your_input_here">```
Tu ne input aisa diya jo "quote tod ke naya attribute inject kare:

```" onmouseover="alert(1)```

### 🎯 Real World Penetration Testing Mein Kab Use Karte Hain?

✅ Jab:

- Input user-controlled ho
- Input directly jaye kisi tag ke attribute ke andar
- < > encode ho ja rahe hoon (toh tu HTML tags nahi inject kar sakta)
- But quotes escape ho rahi ho — matlab attribute injection possible hai

### 🔍 Tu penetration testing mein kya kare:
Jab bhi tu koi search, feedback, comment, ya URL input dekhe jo reflect ho raha ho:

Dekh ke input attribute ke andar to nahi gaya?

Kya quotes properly escape nahi ho rahe?

Tu payload try kar:

```"onmouseover="alert(1)```

Ya advanced:

```"autofocus onfocus=alert(1)```

Trigger hone par samajh ja: Reflected Attribute-based XSS hai 😎

---

Bhai yeh XSS ka aik unique style hai jahan hum HTML ka attribute todh kar apna code inject karte hain.
Penetration testing mein iska faida tab uthta hai jab application user input ko attribute ke andar dalti hai aur usko sanitize nahi karti.
Iss tarah tu session cookies le sakta hai, phishing kara sakta hai, ya redirect kar sakta hai victim ko. Yeh chhoti si injection kabhi kabhi poori website compromise kar deti hai 🔥

