**Arbitrary Object Injection in PHP** — matlab website PHP serialization use kar rahi hai session ke liye aur hum malicious object inject kar ke system ko exploit karenge.

---

### Step-by-Step Solution

1. **Login karna**

   * Credentials use karo: `wiener:peter`
   * Login karte hi ek session cookie milegi (yehi cookie ek serialized PHP object hoga).

---

2. **Cookie inspect karna**

   * BurpSuite me "Proxy → HTTP history" open karo.
   * Cookie decode karke dekho, aapko ek PHP serialized object milega jisme class aur attributes honge.

---

3. **Source code access lena**

   * Site map open karo (Burp → Target tab).
   * Ek file dikhegi: `/libs/CustomTemplate.php`
   * Right-click → "Send to Repeater".
   * Ab URL ke end me tilde (\~) add karo:

     ```
     /libs/CustomTemplate.php~
     ```
   * Ye editor ka backup version de dega jo source code expose karega.

---

4. **Code analysis**
   Source code me ye milega:

   ```php
   class CustomTemplate {
       public $lock_file_path;

       function __destruct() {
           unlink($this->lock_file_path);
       }
   }
   ```

   Matlab jab object destroy hoga (script end hone par), `__destruct()` method call hoga aur `unlink()` function chal jayega jo file delete karta hai.
   **Bingo 🎯 — ye hi vulnerability hai!**

---

5. **Payload banana (malicious object)**
   Ab humein ek object banana hai jisme `lock_file_path` point kare `/home/carlos/morale.txt` par.

   Correct serialized PHP object:

   ```
   O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}
   ```

   Breakdown:

   * `O:14:"CustomTemplate"` → object of class CustomTemplate (14 characters).
   * `s:14:"lock_file_path"` → string attribute, length 14.
   * `s:23:"/home/carlos/morale.txt"` → path string, length 23.

---

6. **Encoding**

   * Isko Base64 encode karo (Burp Decoder ya online tool se).
   * Phir URL encode bhi kar lo (kyunki cookies me special characters break karte hain).

---

7. **Cookie replace karna**

   * Burp Repeater me ek authenticated request bhejo (jaise home page access karna).
   * Jo session cookie hai usko apne malicious Base64 encoded object se replace kar do.

---

8. **Request send karna**

   * Request bhejne par jab server session deserialize karega, to object ka `__destruct()` method trigger hoga aur file delete ho jayegi:

     ```
     /home/carlos/morale.txt
     ```
   * Lab successfully solve ho jayega ✅

---

### ⚡ Real-World PenTesting Tip

Ye attack real websites me tab hota hai jab developers **user input ko directly unserialize** karte hain without validation.

* Tum exploit karke file delete, RCE (remote code execution), ya privilege escalation bhi kar sakte ho.
* Hamesha dekho: `__wakeup()`, `__destruct()`, `__toString()` — ye magic methods sabse dangerous hoti hain.

