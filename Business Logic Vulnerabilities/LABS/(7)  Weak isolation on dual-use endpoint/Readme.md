### ✅ **Step-by-Step Lab Solution**

---

### 🔹 **Step 1: Burp Suite ko ON karo**

* Proxy on karo takay saari traffic capture ho

---

### 🔹 **Step 2: Login as normal user**

* Jaao **login page** pe
* Use karo:

  * **Username:** `wiener`
  * **Password:** `peter`
* Successfully login ho jao

---

### 🔹 **Step 3: Go to change password page**

* Jahan 5 input boxes hain:

  * Email (skip)
  * Username
  * Current password
  * New password
  * Confirm new password

---

### 🔹 **Step 4: Try wrong current password**

* Username ko **wiener** hi rehne do
* **Current password** wrong daal do
* New password aur confirm password same daal do
* Submit karo (browser se) takay error observe ho

✅ Result: "Current password is incorrect" ka error aayega

---

### 🔹 **Step 5: Burp Suite se request intercept karo**

* **Intercept ON** karo
* Wahi form dobara submit karo
* Burp mein request capture hogi

---

### 🔹 **Step 6: Remove current-password parameter**

* Request mein `current-password=xyz` **delete** kar do
* Baaki sab theek rehne do
* Request **forward/send** kar do

✅ Result: Password successfully change ho jaayega (bina current password ke)

---

### 🔹 **Step 7: Try same trick for administrator**

* Page dobara open karo
* Username box mein `administrator` likho
* New password aur confirm password fill karo (jo tum chaho)
* Current password ko kuch bhi daal do (galat)

---

### 🔹 **Step 8: Intercept and Modify again**

* Intercept ON karo
* Form submit karo
* Burp mein request capture karo
* `current-password=xyz` **wala parameter hata do**
* Request **forward** kar do

✅ Result: Admin ka password change ho gaya!

---

### 🔹 **Step 9: Login as administrator**

* Logout karo
* Login page par jao
* Use karo:

  * **Username:** `administrator`
  * **Password:** jo tumne set kiya tha

✅ Result: Successfully administrator login ho gaya

---

### 🔹 **Step 10: Delete carlos**

* Admin panel open karo
* User list mein se `carlos` dhoondo
* Delete karo

---

### 🎉 **Step 11: Lab Solved!**

---

### 🔐 Bonus Tip (Pentesting POV):

Is lab ne bataya:

> Agar backend sirf input fields par depend karta hai (authorization na kare), toh logic flaw ka chance hota hai — aur attacker kisi bhi user ka sensitive action perform kar sakta hai.

---

Aise analysis aur breakdown kaam aayega jab tum bug bounty ya real-world pentests karoge. Keep going bhai 💪
Agar chaho toh main is explanation ka **GitHub note format** bhi bana doon.
