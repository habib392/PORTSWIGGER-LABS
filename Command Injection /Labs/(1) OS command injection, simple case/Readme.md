### 🔎 Lab ka scene

* Website ka **stock checker** feature hai.
* Jab tu product aur store ID bhejta hai, woh internally ek **OS command run** karta hai.
* Iss command main tera input (storeID) bina sanitize hue inject ho raha hai.

---

### 🛠 Step by step solve karna

1. **Burp Suite on karo** aur stock checker pe koi product ka stock check kar.

   * Jaise request kuch aisi hogi:

     ```
     GET /product/stock?productID=3&storeID=1
     ```

2. **Burp main intercept karo** aur `storeID` ka parameter modify karo.

3. Ab injection try karo:

   * Linux main operator `|` use hota hai, matlab *pehle yeh command chalao, phir woh bhi*.
   * Change:

     ```
     storeID=1|whoami
     ```

4. Request forward karo.

5. Response main jo output aayega usme **whoami ka result** bhi dikhayega, jaise:

   ```
   stock: 12
   user: www-data
   ```

---

* Server ne tera input **directly shell command** main embed kar diya.
* Tu ne `|` use karke server ko bola: “pehle original command chalao, phir mera command bhi execute karo.”
* Isi tarah tu `;`, `&&`, `||` bhi try kar sakta hai depending on filter.

