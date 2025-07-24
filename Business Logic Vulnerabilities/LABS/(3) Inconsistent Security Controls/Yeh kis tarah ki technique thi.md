# Technique Type
## 🔍 **Broken Access Control (Specifically: Insecure Direct Role Assignment)**

ya

## 🧠 **Privilege Escalation via Email Domain Trust**

---

### ⚙️ **Technique ka Breakdown:**

Tu ne **normal user** banaya → email update ki to look like an **employee** → server ne **auto role escalate** kar diya = **admin access mil gaya**.

Yeh **privilege escalation** ka ek real technique hai — aur aaj bhi **real-world apps** mein milti hai, especially jab:

* Companies **email domain** pe trust karti hain (e.g., `@company.com`)
* **Role management weak** hoti hai
* Developers **client-side checks** pe zyada bharosa karte hain
* Proper **backend validation** nahi hoti

---

## 🎯 Aaj Kal Kahan Milti Hai?

Haan, **aaj bhi yeh bug milta hai**, lekin mostly:

1. **Startups or small orgs** ki websites mein
2. Internal admin panels (e.g., `/admin`, `/dashboard`)
3. SaaS platforms where team roles depend on email domains
4. Customer Support tools (e.g., Freshdesk clones)
5. Internal HR portals or CRM dashboards

---

## 🛡️ Bachaav ka tareeqa (Defense):

1. Email change par **manual approval** ya **domain verification** lagao.
2. Role assignment backend par ho, **based on internal DB**, not email.
3. Sensitive pages (`/admin`) par **server-side role validation** ho.
4. Test karo: kya email change se role escalate ho raha hai?

---

## 💡 Tip for You as a Pentester:

Jab bhi koi app test karo:

* Email update logic zarur check kar
* Role-based UI access (buttons, panels) ko **manually fuzz kar**
* BurpSuite main `Role`, `isAdmin`, `userType` jese fields dekho requests mein

---

### 🧠 **Simple Summary**:

Website ny ek aisi ghalti ki jiska faida uthakar hum admin panel tak pohanch gaye. Jab hum `/admin` pe gaye toh website ne clearly bata diya ke sirf `@dontwannacry.com` walon ko access hai. Yeh first vulnerability thi — attacker ko hint mil gaya.

Phir hum ny apna normal account banaya or uski email update karke `@dontwannacry.com` daal diya. Server ny verify kiye baghair isay accept kar liya aur humein admin panel access mil gaya. Yeh dusri badi ghalti thi — website ko check karna chahiye tha ke kya email domain valid employee ka hai ya nahi.

---
