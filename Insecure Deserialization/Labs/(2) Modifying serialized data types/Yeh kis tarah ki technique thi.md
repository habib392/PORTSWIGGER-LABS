### 🔎 1. Humein kaise pata chala ki yahan PHP use ho raha hai?

* Jab humne cookie decode ki toh usme `O:4:"User":...` jaisa syntax tha.
* `O` ka matlab hai PHP object. Yeh notation **PHP serialization format** ka hota hai.
* Aur hint bhi diya gaya tha ke PHP 7.x ka behavior assume karo.
  👉 Matlab clear ho gaya ke backend PHP hi use kar raha hai.

---

### 🔎 2. String aur Integer kya the, aur barabar kaise samjh gaya?

* Cookie me ek field tha `access_token`.
* Normally yeh **string** hota: `s:1:"0";` (matlab ek string jisme “0” likha hai).
* Humne ise **integer** bana diya: `i:0;` (matlab number zero).

PHP ke loose comparison (`==`) ke hisaab se:

* `"0" == 0` → true
* `"123" == 123` → true
* `"abc" == 0` → bhi true 😅

👉 Is wajah se system ne “access token valid hai” maan liya.

---

### 🔎 3. Humne kaunsa datatype change kiya?

* Humne `s` (string) ko `i` (integer) me change kiya.
* Aur double quotes hata diye kyunki ab value string nahi, integer hai.
  👉 Matlab `\"0\"` ko `0` bana diya.

---

### 🔎 4. PHP Type Juggling Behavior kya hai?

* **Type juggling** ka matlab hai ke PHP automatically type ko convert karke comparison karta hai.
* Agar ek side number aur dusri side string ho toh string ko number me convert karke compare karta hai.
* Example:

  * `"42" == 42` → true
  * `"042" == 42` → true
  * `"abc" == 0` → true (kyunki “abc” ko number me convert karo toh 0 banta hai).

👉 Yehi **quirk exploit** hua is lab me.

---

### 🔎 5. `==` aur `===` ka kya fark hai?

* `==` → loose comparison (sirf values ko compare karta hai, datatype ignore karta hai).
* `===` → strict comparison (values aur datatype dono same hone chahiye).

Examples:

* `"0" == 0` → true
* `"0" === 0` → false (kyunki ek string hai aur doosra integer).
* `"42" == 42` → true
* `"42" === 42` → false

👉 Agar developer `===` use karta toh hamara attack fail ho jaata.

---

⚡ **Final Samajh:**

* Backend PHP tha → serialization format se pata chala.
* Humne `string` ko `integer` me badal diya.
* PHP ke **type juggling (loose comparison)** behavior ka fayda uthaya.
* Agar developer ne `===` use kiya hota toh yeh bypass possible nahi hota.

---

### 🔎 Tumhe `==` kahan dikhayi nahi diya?

* Haan, bilkul sahi, tumhe kahin bhi response ya cookie me `==` ya `===` likha hua dikhayi nahi diya.
* Lekin **backend code** me (jo developer ne likha hai) kahin pehche session validate karte waqt comparison chal raha hoga.
* Example code kuch aisa ho sakta hai (hume nahi dikh raha, but behavior se samajh aata hai):

```php
if ($session->access_token == $db->access_token) {
    // user authenticated
}
```

---

### 🔎 Hume kaise pata chala ke `==` use hua?

* Agar strict comparison (`===`) hota, toh `\"0\"` (string) aur `0` (integer) kabhi equal nahi hote.
* Lekin jab humne string ko integer me badla (`s:1:\"0\";` → `i:0;`), system ne usse valid maan liya.
* Matlab backend ne loose comparison (`==`) use kiya tha.

👉 **Proof by behavior**: Hamara attack chal gaya = developer ne `==` use kiya.

---

### 🔎 Samajhne ka tareeqa

Tumhe exploit karte waqt ye sochna hota hai:

1. Cookie ek serialized PHP object tha.
2. Humne sirf datatype badla aur system confuse ho gaya.
3. Agar strict check hota toh bypass fail hota.

Yani backend ka code kuch aisa hi hoga, warna exploit possible hi na hota.

Access token asal me session validate karne ka tariqa tha.

Lekin jab humne uska datatype change kar diya, to server ka authentication check bypass ho gaya.

Yani access token ne indirectly hume admin banne ka rasta diya, kyunki backend uski value ko galat tarike se compare kar raha tha.



Access token ek session ka entry pass hota hai jo verify karta hai ke banda valid hai.
Developer ne ise string bana diya aur == use kiya, humne ise integer bana diya.
PHP ne "0" == 0 ko true maana aur hum admin ban gaye.

---

### 🧩 Puri command:

```
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}
```

Yeh **PHP Serialized Object** hai. Matlab data ko ek string form me store kiya gaya hai jise PHP dubara “object” banake samajhta hai.

---

### 🔍 Breakdown:

1. **`O:4:"User":2:`**

   * `O` = Object
   * `4` = class name ki length (yahan `User`)
   * `"User"` = class ka naam
   * `2` = is object ke andar 2 cheezen (properties) hain.

   👉 Matlab ek `User` object hai jismein **2 properties** hain:

   * username
   * access\_token

---

2. **`s:8:"username";`**

   * `s` = string
   * `8` = string ki length (yahan "username" ka length 8 hai)
   * `"username"` = property ka naam

   👉 Matlab yeh pehla property ka naam hai: `username`

---

3. **`s:13:"administrator";`**

   * `s` = string
   * `13` = string ki length (administrator ka length 13 hai)
   * `"administrator"` = property ki value

   👉 Matlab username ki value = `administrator`

---

4. **`s:12:"access_token";`**

   * `s` = string
   * `12` = string ki length (access\_token ka length 12 hai)
   * `"access_token"` = doosra property ka naam

   👉 Matlab doosra property ka naam = `access_token`

---

5. **`i:0;`**

   * `i` = integer
   * `0` = value 0 hai

   👉 Matlab access\_token ki value = **integer 0**

---

### ⚡ Sab mila kar:

Yeh serialized object bol raha hai:

> Ek `User` object hai jismein 2 cheezen hain:
>
> * username = `"administrator"`
> * access\_token = `0` (integer)

---

### 🔑 Ab sawal: Access token ka matlab?

* Normally access\_token ek **random secret string** hoti hai (jaise session ka pass).
* Lekin lab me isse `0` rakha gaya hai.
* Aur kyunki backend me loose comparison (`==`) use hua tha:

  * `access_token (integer 0)` server ki database wali `access_token (string "0")` ke barabar maana gaya.

👉 Aur is tarah authentication bypass ho gaya.

---

### 📝 Summary in your words:

* `O` = object, `s` = string, `i` = integer.
* Yeh object ek user ki info store kar raha hai (username + access\_token).
* Username humne change kiya `administrator`.
* Access\_token ko string se integer banaya.
* PHP ne `"0" == 0` ko true maana aur hume admin bana diya.


---

Hum ny datatype badla matlab s sy string ko hata kr i integer kr diya or ius ny accept kr liya iska matlab ius ky backend (Server) main == use hoo rha tha jo ky website ky developer ki ghalti thi ius ny jab website ki settings server pr upload ki too == daal diya jabky iusay === daalna chahie tha iss wajah sy hum ny string ka username bhi change kiya jaisy wiener ka role ko replace kr ky administrator ka role likh diya string main jis sy admin ka access mil gya

---

### 📌 Yeh Technique Kya Thi?

Yeh technique **Insecure Deserialization + PHP Loose Comparison** thi. Matlab humne serialized object ke andar values aur unke data types modify karke system ko confuse kiya. PHP ka purana comparison system (PHP 7.x aur usse pehle) string aur integer ko barabar samajh leta hai agar value match ho jaaye. Is trick ka use karke authentication bypass kiya jaa sakta hai.

---

### ⭐ Lab ki Khaas Baat

* Sirf value badalne se kaam nahi hota tha → datatype bhi change karna zaroori tha.
* Yeh lab specifically PHP ke **type juggling** behavior (string vs integer comparison) exploit kar raha tha.
* Isme ek chhoti si datatype change se normal user ko **admin access** mil gaya.

---

### 🔑 Main Points

1. Session cookie ek serialized PHP object tha.
2. Username aur access token dono client-side object me store the.
3. Authentication check karne ke liye server ne datatype strict nahi rakha.
4. Loose comparison ka misuse karke admin account access mil gaya.
5. Yeh vulnerability bohot dangerous hai kyunki directly **authorization bypass** allow karti hai.

---

### ❌ Developer ki Ghalti

* Server ne **serialized objects client-side store kiye**, jo kabhi secure practice nahi hai.
* Loose comparison (`==`) use kiya instead of strict comparison (`===`).
* Input aur datatype validation nahi kiya.
* Secure session management implement nahi kiya.

---

### 🔍 Aaj Bhi Milti Hai?

Haan, insecure deserialization aur type juggling aaj bhi real-world me mil jaati hai. Specially PHP aur kuch purane frameworks use karne wali applications me.

---

### ⚡ Developer ki Wajah Se Hoti Hai?

Bilkul. Yeh bug 100% developer ki wajah se hoti hai. Agar developer:

* Strict comparison use karein (`===`),
* Proper input aur datatype validation karein,
* Aur sensitive data (jaise session objects) client-side store na karein,
  …toh yeh vulnerability kabhi na hoti.

---

✅ **Final Note:**
Yeh lab sikhaata hai ke kabhi kabhi coding languages ke chhote quirks (jaise PHP ka type juggling) bohot bade security holes ban jaate hain. Penetration tester ko hamesha datatype aur encoding par focus karna chahiye. 🔥
