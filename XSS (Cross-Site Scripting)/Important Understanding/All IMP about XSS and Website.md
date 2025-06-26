### 🔸 HTML (Structure banata hai)
```<tag>``` → HTML ka element hota hai (jaise ```<div>, <p>, <a>```).

attribute → Tag ke andar hoti hai extra info (jaise href, src, id).

```<!-- comment -->``` → HTML comment hota hai, browser ignore karta hai.

### 🔸 CSS (Style deta hai)
color: red; → Element ka color red kar deta hai.

class="box" → CSS mein .box {} likh ke styling di jaati hai.

style="" → Inline CSS hoti hai, direct tag ke andar likhi jaati hai.

### 🔸 JavaScript (Page ko zinda karta hai)
alert("Hello") → Popup show karta hai.

document.write("text") → Page pe direct text ya HTML likhta hai. (⚠️ XSS sink)

element.innerHTML = "abc" → Kisi tag ke andar ka content change karta hai. (⚠️ XSS sink)

### 🔸 Important JavaScript Sources (jahan se data aata hai)
location.search → URL mein ?search=abc se abc extract karta hai.

location.hash → URL mein #abc se abc milta hai.

document.referrer → Last visited page ka address batata hai.

XSS Basics (Super Simple)
Source = Jahan se input aaya (URL, form, etc)

Sink = Jahan input gaya (JS function, HTML attribute, etc)

Reflected XSS = Input URL se aata hai aur turant page pe reflect hota hai

Stored XSS = Malicious input database mein store hota hai aur har user ko dikhta hai

DOM XSS = JavaScript ke through browser mein hi execute hota hai, server touch nahi hota

### 🔸 Common Payloads (Basic)
```<script>alert(1)</script>```→ Old style (mostly blocked now)

```<img src=x onerror=alert(1)>``` → Modern bypass

```"onmouseover=alert(1)``` → Attribute-based injection

### "String" kya hoti hai?
String ka matlab hota hai text ka tukra — matlab alfaaz ya characters jo quotes ke andar likhe jate hain.

✅ Example:
- 'Hello'
- "Pakistan"
- '123abc'
- 'alert(1)'

Yeh sab strings hain — kyunki inhe ' ' ya " " ke andar likha gaya hai.


