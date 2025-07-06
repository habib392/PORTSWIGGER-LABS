### 📁 Topic: `onerror` in JavaScript for XSS

---

#### ❓ Message:

**"Acha yeh onerror jab hum lagaty hain JavaScript main or XSS execute krny ky liye too kya hota hai"**

---

### 🔥 `onerror` kya hota hai?

`onerror` ek HTML ka event attribute hai jo tab chalta hai jab koi cheez (jaise image ya script) load na ho paaye. Iska faida attacker uthata hai XSS (Cross-site Scripting) attack mein JavaScript execute karne ke liye.

---

### 🔓 XSS Attack Mein Kaise Kaam Aata Hai?

Agar image galat hai (exist nahi karti), to browser `onerror` event ko trigger karta hai — aur JavaScript run ho jaati hai.

#### ✅ Example:

```html
<img src=x onerror="alert(document.cookie)">
```

Jab victim is page ko dekhega, aur image load nahi hogi, to browser automatically alert() chala dega jisme document.cookie show hoga.

---

### 🎯 Victim ke sath kya hota hai? (Step by Step)

1. Hacker ne XSS payload inject kiya hota hai (e.g. comments ya messages mein)
2. Victim woh page kholta hai
3. Browser ko image "x" milti hai — jo load nahi hoti
4. `onerror` trigger hota hai
5. JavaScript execute ho jaata hai — jaise:

   ```js
   alert(document.cookie)
   ```

   Ya koi aur dangerous script: cookie chura lena, redirect, keylogger waghera

---

### 🧠 Common XSS Event Attributes (Milte Julte)

| Attribute     | Trigger Kab Hota Hai          | Error deta hai? | Use in XSS |
| ------------- | ----------------------------- | --------------- | ---------- |
| `onerror`     | Jab image/script load fail ho | ✅               | ✅          |
| `onload`      | Jab page ya element load ho   | ❌               | ✅          |
| `onclick`     | Jab user click kare           | ❌               | ✅          |
| `onsubmit`    | Jab form submit ho            | ❌               | ✅          |
| `onfocus`     | Jab input pe cursor aaye      | ❌               | ✅          |
| `onmouseover` | Jab mouse kisi cheez pe aaye  | ❌               | ✅          |
| `onkeydown`   | Jab koi key press ho          | ❌               | ✅          |

📝 Note: `onalert` koi valid event attribute nahi hota — `alert()` JavaScript ka function hai, HTML attribute nahi.

---

### 🔍 Focus aur Autofocus kya hota hai?

#### 📌 `onfocus`:

Jab user kisi input field ko click karta hai ya tab key se us pe aata hai, to `onfocus` chalta hai.

```html
<input type="text" onfocus="alert('focused')">
```

#### 📌 `autofocus`:

HTML ka attribute hai jo input field ko automatically focus deta hai **page load** pe — bina user ke kuch kiye.

#### ✅ Autofocus + onfocus = Auto XSS Trigger:

```html
<input autofocus onfocus="alert('XSS via focus')">
```

➡️ Page load hote hi JavaScript chalay gi.

---

### ✅ Summary Table:

| Attribute | Action Chahiye? | Auto Trigger?    | Error Based? |
| --------- | --------------- | ---------------- | ------------ |
| onerror   | ❌               | ✅                | ✅            |
| onload    | ❌               | ✅                | ❌            |
| onclick   | ✅               | ❌                | ❌            |
| onsubmit  | ✅               | ❌                | ❌            |
| onfocus   | ✅               | ✅ (if autofocus) | ❌            |
| autofocus | ❌               | ✅                | ❌            |

---

### 🧠 XSS Penetration Testing Tip:

Agar website user input allow karti hai (HTML form ya comment box) aur sanitize nahi karti, to attacker aise payloads daal sakta hai:

```html
<img src=x onerror=alert(document.cookie)>
```

Ya:

```html
<input autofocus onfocus=alert('XSS')>
```

Is se bina victim ke kuch kiye JavaScript chal jaata hai, jo cookie chura sakta hai, keylogger daal sakta hai ya redirect kar sakta hai.

---
