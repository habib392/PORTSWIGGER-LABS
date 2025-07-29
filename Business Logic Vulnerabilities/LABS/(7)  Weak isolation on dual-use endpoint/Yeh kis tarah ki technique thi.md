## 🔍 **Yeh Lab Kya Hai?**

**Lab Title:** Weak isolation on dual-use endpoint
Yeh aik **logic flaw** wali lab hai jahan backend trust karta hai ke user sirf apne hi account ko modify karega — lekin validation nahi karta ke kya user waqai authorized bhi hai ya nahi.

---

## 🧰 **Kis Tarah Ki Technique Use Hui?**

Technique ka naam: **Privilege Escalation via Broken Access Control (IDOR-style logic flaw)**

### 🎯 Summary:

* Server **username** parameter par rely kar raha hai
* **current-password** validate nahi kar raha
* Is wajah se koi bhi attacker `username=administrator` set karke **admin ka password change** kar sakta hai

> ⚠️ No session-based user validation = logic bypass possible

---

## 🔑 **Main Points of the Lab:**

| Point | Explanation                                                                              |
| ----- | ---------------------------------------------------------------------------------------- |
| 1️⃣   | Endpoint same hai for normal user and admin (`/change-password`) = **dual-use endpoint** |
| 2️⃣   | Server trust karta hai `username` field par = **flawed trust model**                     |
| 3️⃣   | `current-password` validate nahi ho raha = **authentication bypass**                     |
| 4️⃣   | Attacker easily admin ka password change kar sakta hai                                   |
| 5️⃣   | Login as admin → access admin panel → delete carlos                                      |

---

## 🧪 **(Pentester Approach):**

### Step-by-step penetration tester soch:

1. ✅ **Login as low-privilege user** (e.g., wiener)
2. 🔍 **Observe functionality**: Password change form dekho
3. 🧠 **Test for validation**:

   * Wrong `current-password` daalo → error observe karo
   * Burp mein request pakro → parameter delete karo
4. 🚩 **Tamper request**:

   * `current-password` remove karo
   * `username` ko kisi aur user ka bana do (admin)
5. 🎯 **Check result**: Agar password change ho gaya toh logic flaw confirmed
6. 🔐 **Login with new credentials**: Dekho access mil raha hai ya nahi
7. 🔥 **Post-exploitation**: Admin panel access lo, actions perform karo (delete user etc.)

---

## ❌ **Ghalti Server ki thi ya Developer ki?**

> 💥 **Developer ki ghalti thi.**

### Kyun?

* Developer ne assume kiya:

  > "Frontend mein username aur current-password hain, toh secure hoga."
* Magar backend ne:

  * **Authorization check nahi kiya**
  * **User session ko match nahi kiya with the username being modified**

**Security ka golden rule:**
**"NEVER trust user input on the client-side. Always enforce security on the server-side."**

---

### ✅ **Lesson for Penetration Testers:**

* Jab bhi kisi form mein `username`, `email`, `user_id`, ya `account_id` dikhai de — **suspicious ho jao**
* Test karo:

  * Kya mein kisi aur user ka data access/modify kar sakta hoon?
  * Kya server session se link check kar raha hai ya nahi?

