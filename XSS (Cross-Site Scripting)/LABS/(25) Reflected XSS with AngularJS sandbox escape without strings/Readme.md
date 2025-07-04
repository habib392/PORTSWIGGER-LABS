## Lab 25: Reflected XSS with AngularJS sandbox escape without strings

---

### Goal:
AngularJS app ke andar sandbox escape karna hai — without using any strings like 'alert' ya "$eval"

Matlab:

' " eval()` sab blocked hai ❌

Tumhein pure AngularJS object traversal aur logic se kaam lena hai ✅

---

### 🧠 Concept Breakdown

AngularJS mein:

Normally sandbox hota hai jo dangerous code run hone se rokta hai

Yeh sandbox tumhein alert(1) jese code run nahi karne deta

But agar tum toString().constructor.fromCharCode(...) jese tareeqon se code likho, toh tum sandbox bypass kar sakte ho — bina quotes ke

---
