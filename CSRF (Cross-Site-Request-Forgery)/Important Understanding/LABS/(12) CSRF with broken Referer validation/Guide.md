Kuch websites Referer header ka check karti hain taake CSRF attacks se bacha ja sake, lekin woh isay itni “simple” tareeqay se check karti hain ke attacker usko easily **bypass** kar leta hai.

### Example 1:

Agar website sirf itna check kare ke:

> "Referer **start hona chahiye** vulnerable-website.com se"

Toh attacker aisi link bana sakta hai:

```
http://vulnerable-website.com.attacker-website.com/csrf-attack
```

Browser ko lagay ga ke yeh `vulnerable-website.com` hai, lekin asal mein yeh `attacker-website.com` hai.

### Example 2:

Agar website sirf itna dekhti ho ke:

> "Referer **contain karta ho** vulnerable-website.com"

Toh attacker link aisa bana dega:

```
http://attacker-website.com/csrf-attack?vulnerable-website.com
```

Ab `vulnerable-website.com` URL mein hai, lekin real domain attacker ka hai.

---

**Note wali baat:**

Burp Suite mein test karte waqt shayad yeh kaam kar jaye, **lekin browser mein test karo toh kaam nahi karega**, kyun?

Kyunkay **modern browsers Referer header se query string hata detay hain** — yeh security feature hai taake sensitive data leak na ho.

### Solution:

Agar tu chah raha hai ke **poora URL including query string Referer mein aaye**, toh apni attack response mein yeh header zaroor add kar:

```
Referrer-Policy: unsafe-url
```

⚠️ *Dhyan rahe: “Referer” galat spelling hoti hai normally, lekin jab “Referrer-Policy” header ka naam likhna ho, toh usmein spelling sahi hoti hai — "Referrer" with double R.*

---

