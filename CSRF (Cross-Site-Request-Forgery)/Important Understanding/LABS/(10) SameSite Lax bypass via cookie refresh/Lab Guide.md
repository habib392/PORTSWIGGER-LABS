## 🛠️ SameSite=Lax Cookie ka Bypass — Naye Cookie ke Saath

Normally agar kisi cookie mein **SameSite=Lax** laga ho, toh wo **cross-site POST request** ke saath **send nahi hoti**.
Yani agar koi attacker dusri site se POST kare, toh browser cookie nahi bhejta — **CSRF block ho jata hai.**

---

### 🔄 Lekin ek exception hai:

Jab bhi koi **nayi cookie** issue hoti hai **jisme SameSite attribute nahi diya hota**, toh **Chrome browser** default mein usko Lax bana deta hai.

Ab Chrome ne SSO (Single Sign-On) systems jaise "Login with Google/Facebook" ko na torhne ke liye ek chhoti **2 minute ki window** di hai.

> **Matlab:** Jab nayi cookie issue hoti hai bina SameSite attribute ke, toh us cookie ke liye **pehle 2 minute tak** Lax rule **enforce nahi hota**.

---

### 🕑 Is 2 minute ke andar kya ho sakta hai?

Agar victim ke browser mein wo nayi cookie issue hui hai, toh attacker **cross-site POST request** bhej sakta hai aur browser **wo cookie bhej dega!**
Yani temporary **CSRF bypass** ho gaya.

---

### ❗Important Point:

Agar cookie explicitly set hui ho jisme likha ho:

```
Set-Cookie: session=abc123; SameSite=Lax
```

toh **2 minute ka rule apply nahi hota**. Sirf un cookies pe apply hota hai jisme SameSite likha hi nahi hota.

---

### 🔓 Attack karne ka tariqa:

Itna precise timing wala attack mushkil hota hai. Lekin agar attacker victim se koi aisa kaam karwa de jisse **nayi session cookie mil jaye**, toh attacker ke paas fresh 2-minute window hoti hai jisme wo CSRF attack kar sakta hai.

#### 👇 Example:

* Victim ne “Login with Google” ka button click kiya.
* OAuth flow complete hua — site ne usko **nayi session cookie** de di.
* Ab uske browser mein fresh cookie hai, SameSite default Lax lagi hai.
* Pehle 2 minute mein attacker cross-site POST bhejta hai — aur cookie bhi chali jati hai.

Result? **SameSite Lax bypass ho gaya**.

---

## 🧠 Cookie Refresh Trigger karna — Without Manual Login

Agar attacker chah raha ho ke **victim ko dobara manually login na karna paday**, lekin usko **nayi session cookie mil jaye**, toh ek trick hoti hai:

> ⚠️ **Top-level navigation** use karo — yani user ko **directly dusri site pe bhejo** (jaise OAuth login page), taake browser apne current OAuth session ke saath nayi cookie le aaye.

Lekin isme ek **problem** hai:

* Jaise hi user ko dusri site pe bhejo, toh wo tumhari attacker site se chala jata hai.
* Phir tum **CSRF attack launch nahi kar sakte**, kyunki victim tumhari site pe wapis nahi aaya.

---

### 🪄 Alternate Trick — New Tab se Refresh

Ek aur tariqa yeh hai ke attacker **new tab open kare login ke liye**, taake:

* **Original page khula rahe**
* Cookie refresh ho jaye new tab se
* Wapis attacker ne original page se CSRF attack fire kar diya

Lekin isme bhi **problem** hai:

> 🔒 Modern browsers **popup tabs block** kar dete hain agar wo **user click ke bina** open ho rahi hoon.

---

### ✅ Solution — JavaScript + Click Event

Agar tu **popup tab open** karna chahta hai OAuth login ke liye, toh **user interaction (click)** ka use karna hoga.

```javascript
window.onclick = () => {
    window.open('https://vulnerable-website.com/login/sso');
}
```

Yeh code tab kaam karega jab **user page pe kahin bhi click karega** — tabhi `window.open()` chalega, warna browser rok dega.

---

### 🧪 Real-life Attack Flow:

1. Victim pehle se logged in hai.
2. Attacker usko apni site pe laya.
3. Victim ne page pe kahin click kiya.
4. Click event ke zariye ek **new tab open hua** — jahan OAuth login redirect hua.
5. Server ne nayi cookie issue ki.
6. Victim original tab mein hi hai — ab attacker same tab se **CSRF attack launch** karta hai.
7. Cookie fresh hone ke wajah se browser ne cookie bhej di — **attack successful** 💥

---
