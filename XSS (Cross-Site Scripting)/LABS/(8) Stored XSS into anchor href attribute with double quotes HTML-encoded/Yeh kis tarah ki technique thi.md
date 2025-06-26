# Yeh kis tarah ki technique thi
Yeh Stored XSS technique thi jo:

**href="..."** attribute ke andar payload inject karti hai.

JavaScript URL scheme (javascript:alert(1)) use karti hai.

Victim ko author name ya kisi link pe click karwana parta hai — tab XSS fire hoti hai.

⛔ Event-based nahi thi, click-based thi.

### ✅ Real website par kaise use kar sakta hoon? (Strategy)
Jab kisi site par tumhe comment form ya user input form mile:

Website, profile, or any URL field dhoondo.

Wahan javascript:alert(1) ya similar payload daalo.

Dekho woh link kaisa render hota hai:

Agar href="javascript:..." waise ka waise show ho raha hai,

Aur click karte hi popup aa jaye,

Toh site vulnerable hai.

💡 Smart tip: Yeh manually check kar sakte ho without Burp, sirf browser se.

---

✅ Iss lab ki khaas baat kya thi?
Sirf Website field unencoded thi — baaqi sab properly sanitized the.

Yeh dikhaata hai ke agar ek single input bhi unprotected ho toh XSS ho sakta hai.

Yeh href attribute target kar rahi thi, jo thoda kam common hota hai script tags ke muqablay.

### ✅ Ajkal ke time mein aisi XSS milti hai?
➡️ Rare hoti ja rahi hai, lekin:

Choti/mid-level sites ya custom CMS waali websites mein ab bhi milti hai.
Especially blogs, forums, comment systems, etc.

💡 Reason: Developers sirf visible input sanitize karte hain, lekin href/src attributes ya onclick jaise attributes ignore kar dete hain.

### Conclusion:
Yeh technique tumhare XSS toolkit mein honi chahiye 💼

Aaj bhi yeh kaam karti hai — especially on less secure or old websites

Sirf javascript: pe rely na karo, advance payloads bhi seekho (e.g., base64, data URI, event handlers)

