## 💣 Web Cache Deception kya hota hai?

Ye ek tarah ka attack hota hai jisme attacker **web cache ko bewakoof banata hai** taake woh accidentally **sensitive data** (jaise profile info ya session info) ko **public cache** mein store kar de. Phir koi bhi banda wohi URL maar kar uss private info tak pohanch sakta hai.

---

## 🧠 Kaise kaam karta hai?

1. Attacker ek **fake URL** banata hai jaise:

   ```
   https://example.com/profile/12345/wcd.css
   ```

2. Victim jab is URL pe jaata hai (maybe phishing se ya kisi link pe click karke), to origin server to actual **profile data** bhej deta hai.

3. Cache server sochta hai ke yeh to `.css` file hai (static file), to yeh cache ho sakti hai.

4. Woh **response ko cache kar leta hai**, ab koi bhi banda yeh URL hit karega to **same sensitive data** milega.

---

## 🧪 Real life example:

Soch, ek shopping website hai jahan:

```
https://shop.com/account/12345
```

Yeh URL tera personal data show karta hai.

Agar attacker isko aise likh de:

```
https://shop.com/account/12345/style.css
```

To agar cache server ko samajh na aaye ke yeh dynamic content hai, to woh teri info ko cache mein daal dega.

Ab attacker wohi URL kholta hai aur **tera personal data mil jaata hai**.

---

## ❌ Web Cache Deception ≠ Web Cache Poisoning

| Topic      | Web Cache Deception            | Web Cache Poisoning                           |
| ---------- | ------------------------------ | --------------------------------------------- |
| **Target** | Sensitive data (stealing info) | Injected malicious content (spreading attack) |
| **Trick**  | Dynamic data ko static dikhana | Malicious content ko cache mein dalna         |
| **Impact** | Private info leak              | Other users ko attack serve hota hai          |

---

## 🔎 Caching kya hoti hai?

Jab hum kisi site ka data dekhte hain (like images, CSS), woh baar baar server se na laana paday is liye **web cache** usay save kar leta hai. Jab agli baar wahi request hoti hai to **directly cache se data milta hai** (fast & efficient).

Yeh kaam mostly **CDNs** karte hain (Cloudflare, Akamai waghera).

---

## 🧩 Attack banane ka tareeqa:

1. **Identify karo** endpoint jo sensitive data return karta hai.
2. Dekho kya origin server aur cache server **URL ko alag tarah se parse karte hain**?
3. Aisi URL banao jisme koi **static file extension ho** (e.g. `.css`, `.js`) jisse cache confuse ho jaaye.
4. Victim se us URL pe request maarwao.
5. Ab attacker wohi URL maarega to cached sensitive data mil jaayega.

---

## 🧰 Tools aur Techniques:

* **Burp Suite + Param Miner** extension se har request mein naya query parameter add karo, taake testing clear ho.
* **Headers check karo**:

  * `X-Cache: HIT` → data cache se aaya hai.
  * `X-Cache: MISS` → data server se aaya hai.
* Response time zyada kam ho to samajh jao cache kaam kar rahi hai.

---

## 🧪 Static extension rules se faida uthana:

1. Socho URL hai:

   ```
   /user/123/profile
   ```

   Ab attacker isko change kare:

   ```
   /user/123/profile/wcd.css
   ```

2. Origin server ignore karega `/wcd.css` aur data de dega.

3. Cache sochta hai ke `.css` file hai, aur isay cache kar leta hai.

---

## 🔧 Burp Scanner aur Web Cache Deception Scanner BApp:

Yeh automatically aise bugs dhoond letay hain jo cache rule ya path mapping ka ghalat faida utha rahay hon.

---

## ✅ Pentesting Tip:

Jab bhi kisi site ka test karo, check karo:

* Kya woh dynamic endpoints mein static extension allow karta hai?
* Kya cache headers theek configure hain?
* Kya tum sensitive pages ko URL ke zariye **cache trick** se access kar sakte ho?

---

## ✅ With Example:

### 💡 1. Jab tu **facebook.com** open karta hai:

> Tu mobile se ek request bhejta hai server ko:
> `GET https://facebook.com`

Server kaam karta hai, HTML, CSS, images ka **response** deta hai.

---

### 🔁 2. Tu page ko **refresh** karta hai:

> Ab cache check karta hai:
> "Yeh cheez toh mere paas already hai, kyu dobara server se loon?"

Agar woh data **static hai**, toh cache se hi de deta hai.
✔️ Tez bhi ho jaata hai
✔️ Server pe load bhi kam

---

### ❓ 3. **Static content kya hota hai?**

> Aisi cheezein **jo baar baar change nahi hoti**
> Jaise:

* Website ka logo
* Design (CSS)
* Fonts
* Background images

✔️ Ye cheezein cache ho sakti hain.

---

### 🔁 4. **Dynamic content kya hota hai?**

> Aisi cheezein **jo user ke mutabiq change hoti hain**

Jaise:

* Tera profile page
* Notifications
* Messages
* Balance info

❌ Inko cache nahi karna chahiye — warna doosre users ka data leak ho sakta hai.

---

## 🎯 Ab tu yeh baat bhi theek samjha:

### ➕ Agar tu URL mein `.css` laga de:

```
https://example.com/profile/123/wcd.css
```

Toh:

* Origin server ignore kar deta hai `.css` (kyunki woh dynamic route samajhta hai)
* Lekin cache server sochta hai:
  "O bhai! Yeh toh `.css` file hai, chalo isay save kar lo!"

⚠️ **Ghalti yahan hoti hai** → Cache dynamic data bhi store kar leta hai **by mistake!**

Phir:

* Victim woh URL open karta hai → uska sensitive data aa jaata hai
* Ab attacker wohi URL open kare → usko bhi wohi sensitive data milta hai!

---

## 📦 `X-Cache` Header ka matlab:

| Header Value       | Matlab                                       |
| ------------------ | -------------------------------------------- |
| `X-Cache: HIT`     | Data **cache se aaya**                       |
| `X-Cache: MISS`    | Data **server se aaya**, cache mein nahi tha |
| `X-Cache: DYNAMIC` | Ye dynamic content hai, cache mat karo       |
| `Origin Server`    | Real server jahan se actual data aata hai    |

✔️ Yeh header **response** mein aata hai, **request mein nahi**.

---

## 📘 Final Simple Example:

1. Tu gaya: `example.com/profile`

   * Server se response aaya (dynamic)
   * Cache ne isay store nahi kiya (sahi)

2. Attacker ne link banaya:
   `example.com/profile/wcd.css`

   * Victim click karta hai
   * Server ne data diya (origin server ne ignore kiya `.css`)
   * Cache ne socha: `.css = static`, chalo store kar lo ❌

3. Ab attacker wohi URL maarta hai:

   * Cache ke paas response pada tha
   * Usko victim ka sensitive data mil gaya! 🚨

---

## ✅ kya seekha?

* Cache sirf static cheezon ke liye hoti hai
* Cache agar galat cheez store kare (jaise dynamic content), to **Web Cache Deception** hota hai
* `X-Cache` header se pata chalta hai data kahan se aaya
* `.css`, `.js` jaise extensions se **cache ko chakkar diya ja sakta hai**

---
