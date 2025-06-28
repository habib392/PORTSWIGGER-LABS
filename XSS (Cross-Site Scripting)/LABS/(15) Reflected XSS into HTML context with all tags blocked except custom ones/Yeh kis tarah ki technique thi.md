# 💻 Yeh kis tarah ki technique thi?

## 🔍 Lab ka Naam:
**Reflected XSS into HTML context with all tags blocked except custom ones**

---

## 🧠 Lab ka Maqsad:
Yeh lab hum se chahta hai ke hum XSS inject karein ek aise page mein jahan:
- **Sab normal HTML tags block** ho jaate hain (jaise <script>, ```<img>```, ```<svg>```)
- Lekin **custom tags** jaise `<xss>`, `<khan>` allowed hain
- Humein aisa payload banana hai jo **alert(document.cookie)** ko fire kare — taake victim ki cookies browser se nikaali ja sakein

---

## 🧪 Working Payload (Exploit server mein daalna hai):

```html```

```<script>```

```location =``` ```'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29+tabindex%3D1%3E#x';```

```</script>```

---

🔁 Bas apne lab ki URL daal dena YOUR-LAB-ID ki jagah
Yehi payload use hoga victim ko XSS dene ke liye ✅

### 🔍 Payload Ki Tashreeh

```<khan id="x" onfocus="alert(document.cookie)" tabindex="1">The King</khan>```

| Cheez           | Matlab                                                                                           |
| --------------- | ------------------------------------------------------------------------------------------------ |
| `<khan>`        | Custom HTML tag — browser ko is tag se koi problem nahi                                          |
| `id="x"`        | Yeh tag ka naam hai, jisko hum URL se bulaayenge                                                 |
| `onfocus="..."` | Jab is element pe focus hoga to yeh JavaScript chalegi                                           |
| `tabindex="1"`  | Is tag ko tab key ya `#x` se focus milne layak banata hai                                        |
| `#x`            | URL ka hissa hai — yeh browser ko keh raha hota hai: "Jaa bhai jahan id='x' ho wahan focus kar!" |

---

### Reflected vs Stored XSS — Farq kya hai?

| Type              | Matlab                                                                                                                                         |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| **Reflected XSS** | Jab tu URL mein payload daalta hai aur turant alert chal jaata hai                                                                             |
| **Stored XSS**    | Jab tu payload kisi comment ya post mein daal ke permanently store kar deta hai — aur jab koi bhi banda wo page dekhta hai, XSS chal jaata hai |

**💬 Example:**

Agar tu comment mein daale:

```<khan id="x"``` ```onfocus="alert(document.cookie)"``` ```tabindex="1">Green</khan>```

Aur wo comment save ho jaaye — to ye stored XSS hai.

---

### ❓ tabindex kya karta hai?
tabindex ka kaam hai HTML element ko keyboard se focusable banana.

Asaan Lafzon Mein:
Browser sirf kuch specific tags pe focus deta hai (``<input>, <button>```, etc.).
Agar tu kisi custom tag (jaise ```<khan>```) ko focus karwana chahta hai — to usmein tabindex="1" zaroor daalna hoga.

### 🔍 tabindex="1" ka matlab kya hai?

- Iska matlab hai ke jab user tab key dabaye, to sabse pehle yeh element pe focus ho.

- Agar kisi tag pe tabindex na ho, to browser uspe focus karega hi nahi.

- tabindex="1" → pehla focus

- tabindex="2" → doosra

- Aur aise hi aage

---
