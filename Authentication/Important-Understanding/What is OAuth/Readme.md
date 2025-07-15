### 🔐 What is OAuth?

**OAuth** ek open standard hai jo apps ko allow karta hai ke wo kisi user ke data tak access lein *bina password liye*.

> Example:
> Jab aap kisi website pe "Continue with Google" ya "Continue with Facebook" pe click karte ho, to OAuth hi kaam karta hai.

---

### ⚙️ Real-World Flow (Step-by-Step):

1. **Authorization Request:**
   Website (jaise example.com) user ko bolti hai:

   > “Google se pucho kya yeh user mujhe access dena chahta hai?”

2. **User Authorization:**
   Google/Facebook aapko apna login form dikhata hai. Aap apna password **Google ko dete ho, website ko nahi**.

3. **Authorization Code:**
   Agar aap "Allow" karte ho, to Google ek temporary code website ko deta hai.

4. **Token Exchange:**
   Website Google ko code ke badle ek **access token** mangti hai.

5. **Access Token:**
   Token milne ke baad website aapke data (jaise email, name) ko access karti hai.
   ✅ **Password kabhi share nahi hota.**

---

### 🧠 Summary:

> **OAuth** user ko allow karta hai ke wo kisi third-party app ko **limited access** de sakay *bina apna password share kiye*.
> Token use hota hai for secure access.

---

### 🧪 Real-world Example:

> Aapne Pinterest pe “Continue with Google” dabaya. Google ne aapse poocha:
> “Kya aap Pinterest ko apna naam, email dena chahtay ho?”
> Aapne haan kiya, Google ne token diya. Pinterest ne token verify kiya aur aapka account bana diya — **bina password ke**.

---

### 🧱 OAuth ke Components:

| Component                | Kaam kya karta hai                        |
| ------------------------ | ----------------------------------------- |
| **Resource Owner**       | Aap (user)                                |
| **Client App**           | Website ya mobile app (jaise example.com) |
| **Authorization Server** | Google/Facebook/etc                       |
| **Resource Server**      | Jahan aapka data pada hai (Google APIs)   |

---

### 🔓 Token Types:

| Type              | Use Case                         |
| ----------------- | -------------------------------- |
| **Access Token**  | Data access ke liye hota hai     |
| **Refresh Token** | Naya token lene ke liye hota hai |

---

### ❗ Common OAuth Attacks in Bug Bounty:

1. **Redirect URI Manipulation**

   * Attacker `redirect_uri` ko apne site pe le jata hai aur token capture karta hai.

2. **Access Token Leakage**

   * Tokens agar localStorage ya URL mein hoon, to XSS se leak ho sakta hai.

3. **Scope Manipulation**

   * Attacker zyada permission waale scope inject kar deta hai.

4. **State Parameter Missing**

   * CSRF attack possible hota hai agar `state` parameter use nahi kiya gaya.

---

### 🛡️ Protection Tips:

* **Always validate redirect\_uri**
* **Use short-lived tokens**
* **Don’t expose tokens in URLs**
* **Use `state` param to avoid CSRF**
* **Set proper scopes**

---

### 🔥 Pro Tip (Bug Bounty):

> Jab bhi OAuth implement dekhain:
>
> * Check karo kya `redirect_uri` developer ne hardcode kiya hai ya attacker set kar sakta hai.
> * Look for open redirects.
> * Look for `scope` manipulation.

---

### 📌 Final Recap:

✅ OAuth = Secure Access Without Password
✅ Token = Limited Power
✅ Attacker ka kaam = Token ya flow hijack karna
✅ Tumhara kaam = Flaw pakar ke bounty kamaana 💸
