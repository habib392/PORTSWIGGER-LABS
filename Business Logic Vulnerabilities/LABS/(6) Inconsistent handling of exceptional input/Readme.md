## 📘 Lab: Inconsistent handling of exceptional input

**Level:** Practitioner
**Goal:** Admin panel ka access lena aur `carlos` user ko delete karna

---

### 🧠 **Lab ka Overview**

Is lab mein website ka **registration system email address ko sahi tarah validate nahi karta**.
Hum is vulnerability ko exploit karte hain **admin access lenay ke liye** — sirf email address mein clever input daal ke.

---

## 🪜 Step-by-Step Solution (Meri Zuban Mein)

### 🔹 1. Lab Open Kiya (Burp Suite chala ke)

Jaise hi lab open kiya, maine **"Target > Site Map"** mein jaa ke content discovery chalaya — Burp Suite ka built-in feature.

Us se mujhe yeh path mila:

```
/admin
```

Jab browser mein `/admin` open kiya to yeh error aaya:

```
Only users from DontWannaCry can access this page.
```

➡️ Matlab sirf `@dontwannacry.com` email wale log admin panel dekh saktay hain.

---

### 🔹 2. Email Client Open Kiya

Lab banner se **email client** open kiya aur apna email domain dekha:

```
@exploit-0a3a003a036158b882452d720132005c.exploit-server.net
```

---

### 🔹 3. Pehla Account Register Kiya (Testing)

Maine pehli baar randomly register kiya:

* **Email:** `aaaa@exploit-server.net`
* **Username / Password:** kuch bhi random
* **Email Confirmation:** link par click karke account activate

**My account** page par gaya aur dekha ke email bilkul theek store hui hai.

---

### 🔹 4. Phir Real Exploit Planning Ki

Mujhe idea tha ke **email address backend mein truncate hota hai**
Toh maine calculate kiya:

> `@dontwannacry.com` ke `"m"` letter ko **255th character** pe aana chahiye

---

### 🔹 5. Second Registration (Exploit Attempt)

Maine dusra account banaya:

* **Email:**
  `aaaaaa...(238 characters)...aaa@dontwannacry.com.exploit-server.net`

Jis tarah backend kaam karta hai:

* Woh sirf pehle **255 characters store karta hai**
* Is wajah se backend ko yeh email dikhta hai:

  ```
  abc@dontwannacry.com
  ```

➡️ **Yani system ko lagta hai ke mai company ka employee hoon!**

Maine email confirmation link se account activate kiya.

---

### 🔹 6. Admin Panel Unlock Hogaya ✅

Login karte hi **Admin panel visible** hogaya
Wahan gaya aur `carlos` ko delete kar diya
✅ **Lab successfully solved**

---

## 📚 Extra Knowledge: Truncate Kya Hota Hai?

### 🔤 Simple Definition:

> Truncate ka matlab hota hai **kisi input ko cut kar dena agar woh limit se zyada ho jaye**

---

### 🔧 Real Example:

Database mein agar column ho:

```sql
email VARCHAR(255)
```

Aur user yeh email de:

```
300 characters ka email address
```

Toh system sirf:

```
pehle 255 characters
```

store karega — baqi sab **truncate** (cut) ho jayega bina error diye.

---

### 🛠 Real-Life Pentesting Example:

Agar tum yeh email do:

```
aaaaaa...(long string)...@paypal.com.attacker.net
```

Toh server sochta hai ke tum `@paypal.com` se ho (kyunki `@paypal.com` truncate hoke dikh raha hai)
➡️ Tum access le sakte ho **high privilege area** ka!

---

## 🧪 Pentesting Lesson Learned:

* Hamesha backend storage aur validation ko observe karo
* Kabhi kabhi application logic sirf **string match** par depend karti hai
* Truncation bugs ko **authorization bypass** mein use kiya ja sakta hai

---

## 📝 Summary:

| Step | Action                                                      |
| ---- | ----------------------------------------------------------- |
| 1️⃣  | Pehle account register karke truncation behavior samjha     |
| 2️⃣  | Email address carefully craft kiya with `@dontwannacry.com` |
| 3️⃣  | Email truncate hone ke baad system ko confuse kiya          |
| 4️⃣  | Admin panel unlock hua, `carlos` ko delete kiya             |

