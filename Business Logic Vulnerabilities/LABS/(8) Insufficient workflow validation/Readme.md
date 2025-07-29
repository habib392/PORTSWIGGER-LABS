## Insufficient workflow validation

### 🔐 **Solution in Simple Steps**:

#### 🛠 Step 1: BurpSuite ko chalao

* Burp ko **intercept off** karo aur proxy history record hone do.

---

#### 👤 Step 2: Login karo

* Website par login page jao
* Use these credentials:
  `Username: wiener`
  `Password: peter`

---

#### 🛍 Step 3: Koi sasti cheez khareedo jo credit mein aa jaye

* Jaise koi **cheap product** jiska price tumhare **store credit** se kam ho
* Add to cart karo
* "Place order" karo

---

#### 🔍 Step 4: Proxy History mein yeh request dhoondo:

```
POST /cart/checkout
```

* Yeh woh request hai jo order place karte waqt jaati hai

---

#### 📌 Step 5: Order confirmation URL dhoondo

* Iske baad browser tumhe redirect karega is URL par:

```
GET /cart/order-confirmation?order-confirmation=true
```

---

#### 🔁 Step 6: Is `GET /cart/order-confirmation...` request ko Burp Repeater mein bhejo

---

#### 🧥 Step 7: Ab expensive jacket (Lightweight l33t leather jacket) add karo cart mein

* Product page pe jao
* **Leather jacket** add to cart karo
* Lekin checkout mat karo!

---

#### 🧠 Step 8: Repeater se wohi **order confirmation** request dubara send karo

* **Jo tumne pehle save ki thi:**
  `GET /cart/order-confirmation?order-confirmation=true`

---

#### 🎯 Step 9: Dekho kya hota hai

* Order place ho jata hai **leather jacket ka**,
* **Credit deduct nahi hota**
* Lab solve ho jata hai ✅

---

### 💡 Is lab ka core flaw kya tha?

> Website assume kar rahi thi ke order-confirmation page sirf tabhi access hoga jab user checkout karega — lekin **GET request directly bhej kar** hum uss step ko bypass kar gaye.

---

### 🛠 Penetration Testing Tip:

Jab bhi e-commerce site test karo:

* Dekho kya **checkout process** properly validate ho raha hai?
* Kya koi URL **directly access** karne se workflow break ho raha hai?
* Kya item delivery, payment ya stock system se **asynchronously** linked hai ya nahi?

