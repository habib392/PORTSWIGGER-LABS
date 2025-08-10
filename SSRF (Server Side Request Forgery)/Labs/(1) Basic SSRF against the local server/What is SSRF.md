## **Server-Side Request Forgery (SSRF)**

### **1. Definition**

**Server-Side Request Forgery (SSRF)** ek web security vulnerability hai jisme attacker server ko apni marzi ke requests bhejne par majboor karta hai.
Yeh requests un locations par ja sakti hain jo normal users ke liye accessible nahi hoti, jaise:

* Organization ke internal network services
* Private APIs
* Sensitive admin panels

---

### **2. Root Cause**

SSRF tab hoti hai jab:

* Server backend user ke input ko directly use karke dusre servers ya URLs ko request bhejta hai
* Aur input ko sahi tareeke se validate ya sanitize nahi karta

---

### **3. Real-World Analogy**

Socho tum kisi ko phone number dete ho taake wo sirf tumhare dost ko call kare, lekin wo number badal ke kisi secret agency ko call kar le.
SSRF mein yahi hota hai — server ko galat jagah contact karwaya jata hai.

---

### **4. Example (Shopping Website)**

Ek shopping app stock status check karne ke liye backend API se connect karti hai:

**Normal Request:**

```
stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=6&storeId=1
```

**Attacker Modified Request:**

```
stockApi=http://localhost/admin
```

Result:

* Server apne localhost ke `/admin` panel ko access karega
* Localhost se aayi request ko server trusted samajh kar admin access de dega

---

### **5. Example (Amazon + Cloudflare)**

* Amazon ke backend servers bahut saare internal APIs ko call karte hain
* Agar URL user input se aata hai aur filter nahi hota, attacker us URL ko badal ke:

```
http://internal-api.amazon.com/admin
```

bhej sakta hai

* Cloudflare network ko protect karta hai, lekin backend ke andar jo request ban rahi hai, wo directly internal systems tak pohanch sakti hai

---

### **6. Impact**

Successful SSRF se attacker:

* Internal data access kar sakta hai
* Backend admin panels use kar sakta hai
* Internal APIs se sensitive info nikal sakta hai
* Commands execute kar sakta hai (RCE in some cases)
* Organization ke naam se external servers par attack launch kar sakta hai

---

### **7. Types of SSRF Attacks**

#### **a) SSRF Against the Server**

Server apne hi loopback interface (`127.0.0.1` / `localhost`) pe request bhejta hai.

#### **b) SSRF Against Other Systems**

Server ke network ke andar ke doosre systems ya APIs ko target karna.

---

### **8. Difference Between SSRF & SQL Injection**

| Feature | SQL Injection                 | SSRF                                 |
| ------- | ----------------------------- | ------------------------------------ |
| Target  | Database queries              | HTTP/Network requests                |
| Goal    | Database data manipulate/read | Internal/external resources access   |
| Cause   | Unfiltered input in SQL code  | Unfiltered input in request handling |
| Fix     | Query sanitization            | URL validation, allowlist            |

---

### **9. Prevention**

* **Allowlist**: Sirf allowed domains/hosts ko request karne do
* **Input Validation**: URL scheme, host, port check karo
* **Block Internal IP Ranges**: `127.0.0.1`, `10.x.x.x`, `192.168.x.x` block karo
* **Use Metadata API Protection** (e.g., AWS metadata endpoints block)
* **Don’t trust user-controlled URLs** in backend requests

---

* SSRF **sirf shopping websites** mein nahi hoti — Facebook, YouTube, Amazon, kisi bhi website mein ho sakti hai
* Vulnerability **server side** mein hoti hai, jab server input ko validate nahi karta
* Cloudflare jaise services front-end traffic filter karti hain, lekin backend validation ki kami SSRF ka root cause hai
* Har SSRF ka root reason hai **developer ka user input par blind trust**

---
