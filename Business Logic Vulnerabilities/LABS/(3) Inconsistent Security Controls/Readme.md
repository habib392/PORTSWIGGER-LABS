**Admin panel sirf authorized users (DontWannaCry employees) ke liye hona chahiye**, lekin app sirf email domain check karke access de rahi hai — **yeh flawed logic hai**.

---

## 🪜 Step-by-Step Simple Guide:

1. **BurpSuite khol kar** → `Target > Site map` → Lab domain par right-click karo →
   `Engagement tools > Discover content`.

2. Session chalao aur dekho `/admin` path milta hai.

3. Browser main `/admin` jao → Error aata hai: "Only DontWannaCry users can access".

4. 🧠 **Hint mil gaya**: Access dena sirf `@dontwannacry.com` walon ko milta hai.

5. **Account banao**:

   * Email use karo: `anything@<your-lab-domain>.web-security-academy.net`
   * Email client main jao → confirmation link click karo.

6. Account login karo → `My account` page pe jao → email change karo to:
   **`abc@dontwannacry.com`**
   (Kuch bhi likh lo `@dontwannacry.com` se end hona chahiye)

7. Ab `/admin` page dubara open karo → access mil jayegi ✅

8. **Carlos ko delete karo** → 🎉 **Lab solved!**

---

## 💥 Real-World Penetration Testing Tip:

Aisi flaws real apps main hoti hain jab:

* Email domain ko identity ka base banate hain.
* Role check nahi hota backend par.

**Always test**:

* Email change logic
* Role elevation after registration
* Authorization bypass on sensitive URLs like `/admin`, `/billing`, `/settings`
