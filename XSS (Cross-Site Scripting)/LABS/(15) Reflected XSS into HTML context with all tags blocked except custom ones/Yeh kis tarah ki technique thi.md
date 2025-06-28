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
Browser sirf kuch specific tags pe focus deta hai (```<input>, <button>```, etc.).
Agar tu kisi custom tag (jaise ```<khan>```) ko focus karwana chahta hai — to usmein tabindex="1" zaroor daalna hoga.

### 🔍 tabindex="1" ka matlab kya hai?

- Iska matlab hai ke jab user tab key dabaye, to sabse pehle yeh element pe focus ho.

- Agar kisi tag pe tabindex na ho, to browser uspe focus karega hi nahi.

- tabindex="1" → pehla focus

- tabindex="2" → doosra

- Aur aise hi aage

---

### 🧠 #x kya karta hai?
Agar URL ke end mein tu likhta hai #x, to browser us element ko focus karta hai jiska id="x" hai.

Jaise:

```<khan id="x"```
 ```onfocus="alert(document.cookie)"```
 ```tabindex="1">The King</khan>```

Aur tu URL mein likhe:

```https://example.com/page?search=....#x```

To browser page load hote hi direct us tag pe focus karega → onfocus chalega → alert boom 💥

---

### ⚠️ Mouse ka koi taalluq nahi
Tu ne pucha tha ke kya mouse se kuch hoga?
Nahi bhai — mouse le jaane se onfocus nahi chalta.

Ye sirf chalta hai:

- Tab key dabane se

- URL mein #x dene se

- JavaScript se .focus() method se

---

### ❌ Kya spaceindex, ctrlindex hota hai?

Nahi bhai! Browser sirf tabindex hi jaanta hai
Koi aur index browser ko samaj nahi aata.

Agar tu space ya ctrl key se koi function chalana chahta hai, to uske liye use kar:

```onkeydown / onkeyup```

**Example:**

```<khan onkeydown="if(event.key===' ') alert('Space dabaya!')" tabindex="1">Green</khan>```

---

### Real Life Pentesting mein faida kaise?

Jab tu koi site test kare aur dekhe ke ```<script>``` block ho raha hai, lekin custom tags allow hain — to ye XSS technique kaam aayegi

Tu comment, search box, ya feedback forms mein ye try kar sakta hai

Bug bounty mein ye trick kabhi kabhi filter bypass karne mein madad karti hai

### 🛠️ Step by Step Strategy:
Dekh koi input field ya search area hai jo input ko page pe dikhata ho

Wahaan yeh inject karo:

```<khan id="x" onfocus="alert(document.cookie)" tabindex="1">Test</khan>```

Fir URL mein #x laga ke page open karo:

https://site.com/page?search=...#x
Page load hote hi browser focus karega id="x" wale tag pe → onfocus chalega → XSS success!

---

Yeh lab ne humein sikhaya ke agar normal HTML block ho rahi ho, to bhi XSS ka tareeqa hota hai — bas dimaag chalayen aur custom tag, tabindex, aur focus ka sahi use karein!

---

🧬 Source & Sink Analysis:

🟢 Source (input kidhar se aa raha hai)?

✅ location.search → Matlab URL ka ?search=... part


Example:

```site.com/?search=<xss+onfocus=alert(1)+tabindex=1>#x```

---

### 🟢 Sink (browser uss input ko kaise inject kar raha hai)?

✅ innerHTML — kyunki input HTML ke form mein reflect ho raha hai

InnerHTML allow karta hai ke tag + attributes page pe render hon → isi wajah se XSS chalta hai.


