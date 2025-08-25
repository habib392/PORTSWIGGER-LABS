### 🔎 Kab aur kahan test karna hai insecure deserialization?

1. **Cookies (Session / Auth token)**

   * Agar cookie **base64, hex ya JSON** me encoded lagti hai → decode karke dekho.
   * Agar andar kuch **object format (O:.. in PHP ya rO0AB.. in Java)** mile → samajh jao serialization chal rahi hai.

2. **Hidden fields / form parameters**

   * Kabhi developers object serialize karke **hidden form field** me bhej dete hain (jaise cart object, profile settings).

3. **API request bodies**

   * APIs me specially JSON me object hota hai, lekin kabhi kabhi devs binary ya base64 encoded serialized object bhejte hain.

4. **File uploads / export-import features**

   * Agar app tumhe koi “profile export” ya “report import” feature de rahi hai → usme bhi serialized objects mil sakte hain.

---

### 🛠️ Kaise identify karna hai?

* **PHP serialization** → string kuch aisa hota hai:

  ```
  O:8:"UserData":2:{s:4:"name";s:5:"Habib";s:3:"age";i:19;}
  ```
* **Java serialization** → hamesha start hota hai `rO0AB` se (base64 decode karne ke baad).
* **.NET serialization** → mostly XML format me milta hai `<DataSet>` ya `<System.Data...>`.
* **YAML / JSON** based serialization bhi hoti hai (rare cases).

---

### ⚠️ Sirf base64 decode karna hi kaafi nahi

* Hamesha base64 decode check karna sahi hai, lekin kabhi payload **plain text** me bhi ho sakta hai (e.g. PHP objects bina encode).
* Kuch jagah developers **double encoding** karte hain (pehle serialize, phir base64, phir URL encode).

---

### 🔑 Pentesting mindset

1. **Dekho** → kya data client side se server ko ja raha hai?
2. **Socho** → kya yeh data directly object ban sakta hai server pe?
3. **Check karo** → decode karke serialization pattern dikh raha hai kya?
4. **Exploit karo** → phir PHPGGC, ysoserial, ya custom gadget chain se payload banao.

---

👉 Matlab simple: tumhe **cookie, hidden field, API body, ya file** me encoded ya object-like data mile → turant suspicion rakho ke shayad deserialization ho rahi hai.

