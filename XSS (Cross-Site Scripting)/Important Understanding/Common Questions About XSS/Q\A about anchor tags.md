# Q/A

## 💡 Scenario

Agar kisi website ka URL yeh ho:

```https://www.example.com/<script>alert(1)</script>```

Aur jab inspect element mein tu dekhe:

```<a href="https://www.example.com/<script>alert(1)</script>">```

To tu sochta hai:

- Kya meri input HTML ke anchor tag ke andar ja rahi hai?
- Kya JavaScript run ho rahi hai?
- Kya XSS ka chance hai?


### ✅ Sahi Soch Kya Hai?

🔸 1. Anchor tag mein <script> likhne se JavaScript run nahi hoti.

<script> tag href attribute ke andar kaam nahi karta.

Browser isse ya to encode kar deta hai, ya ignore karta hai.

Isliye ye payload usually execute nahi hota:

```<a``` ```href="https://example.com/<script>alert(1)</script>">Click</a>```

---

🔸 2. Agar tu chahta hai ke anchor tag se JavaScript chale...

To correct payload hota hai:

```<a href="javascript:alert(1)">Click me</a>```

Aur agar input reflect ho raha ho href mein:

```https://site.com/page?link=javascript:alert(1)```