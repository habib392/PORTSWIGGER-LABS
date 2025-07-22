## Lab: Excessive trust in client-side controls

Website developer ne assume kiya ke user front-end (browser) ka data kabhi nahi badlay ga. Lekin burp jaisay tools se hum dekh saktay hain ke request mein "price" client-side se aata hai — aur server bina verify kiye use maan leta hai. **Yehi logic flaw hai.**

---

## ✅ Step-by-step Lab Solution (BurpSuite ke sath):

### 🔹 1. Login

* Open the lab link.
* Login with:

  ```
  Username: wiener
  Password: peter
  ```

---

### 🔹 2. Add Jacket to Cart

* "Lightweight l33t leather jacket" ko cart mein add karo.
* Burp ka **Proxy > HTTP history** section open karo.
* `POST /cart` wali request dhoondo — **usmein "productId" ke sath "price" bhi hoga.**

---

### 🔹 3. Send Request to Repeater

* `POST /cart` wali request ko **right click** karke **"Send to Repeater"** karo.

---

### 🔹 4. Change the Price (Exploit)

* Burp Repeater mein:

  * Price ko **kam** karo (jaise `10` ya `5`)
  * Example:

    ```
    price=5
    ```
* Send the request.
* Browser mein wapas jaake **cart refresh** karo — dekhna price change ho gaya hoga.

---

### 🔹 5. Buy the Jacket

* Ab price tumhari available **store credit** se kam hai.
* Proceed to checkout aur **order complete** karo.
* Agar sahi kiya to lab solve ho jayega.

---

## 🧪 Penetration Testing Point:

Ye flaw real life mein bhi hota hai jab:

* Developers client-side price pe bharosa karte hain
* Server-side validation nahi hoti

**As a pentester**, tum hamesha:

* Cart/payment requests mein `price`, `discount`, `coupon`, `totalAmount` jaise parameters dhoondo
* Unko burp repeater ya intruder mein test karo

---
