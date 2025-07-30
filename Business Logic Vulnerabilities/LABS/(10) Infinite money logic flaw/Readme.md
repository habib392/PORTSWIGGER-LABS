## 💸 Lab: Infinite Money Logic Flaw

**🧪 Target: Lightweight l33t leather jacket ko store credit se buy karna by abusing business logic flaw**

---

## 🪜 **Step-by-step solution (Roman Urdu):**

### 🟢 Step 1: Login karna

1. Burp Suite on karo
2. Lab open karo aur **login page** pe jao
3. Credentials daalo:

   ```
   Username: wiener  
   Password: peter  
   ```

---

### 🟢 Step 2: Gift card buy karna

1. Login ke baad **Gift Cards** page pe jao
2. `$10` wala gift card "Add to Cart" karo
3. Neeche scroll karke **Newsletter signup** section mein jao
4. Random email daalo (example: [test@attock.com](mailto:test@attock.com))
5. Tumhe ek coupon code milega (example: `SIGNUP30`)

---

### 🟢 Step 3: Checkout aur coupon apply karna

1. **Cart** open karo aur checkout page pe jao
2. `SIGNUP30` coupon code apply karo
3. Gift card price `$10` se kam ho jaayegi (`$7`)
4. Ab payment complete karo — tumhe ek **gift card code** milega
5. Is gift card code ko **copy** kar lo

---

### 🟢 Step 4: Gift card redeem karna

1. **My Account** page pe jao
2. Neeche **"Redeem gift card"** box mein gift card code paste karo
3. Redeem karte hi tumhare account mein `$10` credit add ho jaayega

---

### 🟢 Step 5: Burp mein Macro automation setup karna

#### 5.1 — HTTP history se 5 requests select karo:

* `POST /cart`
* `POST /cart/coupon`
* `POST /cart/checkout`
* `GET /cart/order-confirmation?order-confirmed=true`
* `POST /gift-card`

#### 5.2 — Macro banana:

1. Burp → **Settings** → **Sessions**
2. Click `Add` to create new rule
3. `Scope` tab pe jaa kar "Include all URLs" select karo
4. `Details` tab → Add → Run a Macro
5. Macro recorder mein upar wali 5 requests select karo
6. OK karo → Macro editor open hoga

#### 5.3 — Gift card extract karna:

1. `GET /cart/order-confirmation?...` request pe click karo
2. Configure item → Add parameter → Name: `gift-card`
3. Response ka gift card code highlight karo
4. OK karo

#### 5.4 — Macro mein gift-card parameter set karna:

1. `POST /gift-card` request pe click karo
2. Configure item → Parameter handling → Use `gift-card` from **response 4**
3. OK karo

#### 5.5 — Macro test karna:

1. Test Macro pe click karo
2. Dekho kya `302` response mil raha hai
3. Agar haan → Macro successfully automate hogaya

---

### 🟢 Step 6: Burp Intruder se loop chalana

1. `GET /my-account` ko right-click karo → **Send to Intruder**
2. **Attack Type**: Sniper
3. **Payloads**: Null payloads select karo → Generate 412 payloads
4. **Resource Pool**:

   * Add new pool
   * Max concurrent requests: `1`
5. **Attack Start** karo
6. Jab attack khatam ho jaaye, tumhare paas enough store credit ho jaayega jacket buy karne ke liye

---

## ✅ Final Step: Jacket buy karo

* Store credit se **Lightweight l33t leather jacket** buy karo
* Lab solve ho jaayega ✅

---

## 💥 Business Logic Flaw Samajh lo:

> Tum 7\$ mein gift card khareed rahe ho, aur use karke 10\$ kama rahe ho
> Har dafa 3\$ ka profit → Infinite money glitch 💸
> Yehi hoti hai business logic vulnerability

