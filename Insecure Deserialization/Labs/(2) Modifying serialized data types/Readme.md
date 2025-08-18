## **Lab: Modifying Serialized Data Types**

### Step 1 – Login aur Request Capture karo

1. Website par login karo `wiener : peter` credentials se.
2. BurpSuite main jao aur **GET /my-account** request ko dekh lo (login ke baad).
3. Wahan ek **session cookie** milegi jo Base64 + URL encoded hogi.

---

### Step 2 – Cookie Decode aur Inspect karo

1. Request ko **Repeater** main bhejo.
2. Inspector → cookie decode karo.
3. Tumhe ek serialized PHP object milega jaisa:

   ```
   O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"KuchRandomToken";}
   ```

---

### Step 3 – Modifications karo (yahan trick hai ⚡)

1. Username ko `administrator` banana hai.

   * Ab length bhi sahi karni padegi → `s:13:"administrator";` (kyunki "administrator" 13 characters ka hai).
2. Access token ko integer banana hai → string nahi.

   * Matlab pehle `s:32:"randomstring";` tha.
   * Ab ise likhna hai `i:0;` (integer 0).
   * Dhyaan rahe → quotes hata do aur `s` ko `i` se replace karo.

Final object kuch aisa banega:

```
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}
```

---

### Step 4 – Request Send karo

1. Burp repeater main **Apply changes** kar do.
2. Request send karo.
3. Response main tumhe **/admin** ka link dikh jaayega.

---

### Step 5 – Admin Panel Access karo

1. Ab request ka path change karo:

   ```
   GET /admin HTTP/2
   ```
2. Request bhejo. Response main Carlos delete option dikh jaayega.

---

### Step 6 – Carlos Delete karo

1. Ab ek aur request banao:

   ```
   GET /admin/delete?username=carlos HTTP/2
   ```
2. Request send karo. Response aayega → **LAB SOLVED ✅**

---

## ⚡ Samajhne Wali Baat

* Yeh vulnerability PHP ke **loose comparison behavior** ki wajah se hoti hai.
* PHP `==` operator use karta hai, aur jab string `"0"` aur integer `0` compare hote hain → equal ho jaate hain.
* Developer ne access token ka datatype verify nahi kiya → is wajah se bypass ho gaya.

---

## 🔑 Main Points

1. Cookie ek **serialized PHP object** thi.
2. Username aur datatype manually edit kiya.
3. PHP ka loose comparison exploit hua.
4. Admin ban gaye aur Carlos delete kar diya.
