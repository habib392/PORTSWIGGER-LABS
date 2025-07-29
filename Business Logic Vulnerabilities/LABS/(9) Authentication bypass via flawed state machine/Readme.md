## 🧠 **Concept Samajho (First Principles)**

Developer ne yeh assume kiya hai ke login ka flow hamesha yeh hoga:

1. Login →
2. Role select →
3. Home page

Lekin agar hum **role select** step ko skip kar dein, toh server galti se **default role** assign kar deta hai, aur wo **admin** hota hai 😎
Yeh hota hai **flawed state machine logic**.

---

## ✅ **Lab Solve Karne ke Steps (Tumhare Hisab se)**

### 🔹 Step 1: Normal Login Flow Dekhna

* Login page par gaya.
* Credentials daalay: `wiener:peter`
* Login kiya → Role select karne ka option aaya.
* Role select kiya → Homepage open ho gaya.
* `/admin` URL manually daalne par **Access Denied** error aayi.

---

### 🔹 Step 2: Exploit Start Karna (BurpSuite Use Karna)

* Logout kiya.
* **BurpSuite ka intercept ON** kiya.
* Dubara `wiener:peter` se login kiya.

---

### 🔹 Step 3: Requests Handle Karna

* `POST /login` wali request ko **Forward** kar diya.
* Uske baad jo `GET /role-selector` request aayi, usko **Drop** kar diya.

---

### 🔹 Step 4: Bypass karna

* Browser mein error aaya (kyunki role-selector drop ho chuka tha).
* Tab URL copy kiya aur naye tab mein paste kiya.
* **Burp ka intercept OFF** kiya.
* Dekha ke homepage open ho gaya **aur admin panel bhi visible hai**.

---

### 🔹 Step 5: Final Step

* **Admin panel** mein gaya.
* Wahan se **carlos ko delete** kar diya.
* ✅ **Lab Solved!**

---

## 🧠 Penetration Testing Tip:

**Flawed state machines** ka matlab hota hai ke developer sirf correct sequence check karta hai, lekin agar koi step **skip** ya **tamper** kare toh server default ya ghalat behavior dikhata hai — ye kaafi common logic flaw hai.

**Real-world example**: Agar kisi bank app mein verification skip karne se user admin ban jaye — that's critical!

