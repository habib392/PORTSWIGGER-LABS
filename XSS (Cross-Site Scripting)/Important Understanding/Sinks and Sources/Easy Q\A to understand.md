Jab main Pakistan search krta hoon to location.search se data nikalta hai aur document.write(Pakistan) mein chala jata hai

---

**Q:** Agar "Pakistan" ke liye location.search use ho raha hai, to agar koi aur cheez jaisy "cat", "movie", ya "Islamabad" search karen to kya source badal jata hai?

✅ Short Answer:
Nahi bhai, source badalta nahi.
Input jo bhi ho — word, animal, movie — agar wo URL ke through jaa raha hai, to location.search hi source hota hai.

---

Agar developer ne input sanitize kar diya ho, toh woh dangerous characters ko safe bana deta hai.
Jaise < > " ' / etc.

🔐 Example of sanitized input:

Tum payload bhejte ho:

```?search=<script>alert(1)</script>```

Lekin page pe dikhai deta hai:

```&lt;script&gt;alert(1)&lt;/script&gt;```

Yeh characters encoded ho gaye:

< ➝ &lt;

> ➝ &gt;

➡️ Iska matlab: JavaScript nahi chalaygi
➡️ Matlab: Sanitization ho chuki hai ✅

--- 

