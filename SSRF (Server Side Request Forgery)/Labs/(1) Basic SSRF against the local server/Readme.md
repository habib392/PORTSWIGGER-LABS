## **Basic SSRF against the local server — Step by Step**

**Scenario:**
Website ka “Check stock” feature backend se ek URL call karta hai jo product ka stock check karta hai. Hum isko modify karke backend ko `http://localhost/admin` pe bhejenge.

---

### **1. Direct Admin Access Check karo**

* Browser me `http://<lab-url>/admin` open karo
* Yeh access deny karega (kyun ke tum directly authorized nahi ho)

---

### **2. Product ka Stock Check karo**

* Lab me koi product open karo
* “Check stock” button pe click karo
* Burp Suite ka **Proxy ON** hona chahiye

---

### **3. Burp me Request Intercept karo**

* Request aayegi kuch is type ki:

```
POST /product/stock HTTP/1.1
Host: <lab-host>
Content-Type: application/x-www-form-urlencoded

stockApi=http://stock.weliketoshop.net/product/stock/check?productId=1&storeId=1
```

---

### **4. Request ko Repeater me bhejo**

* **Right Click → Send to Repeater**

---

### **5. URL ko Modify karo**

* `stockApi` ka value badal ke:

```
stockApi=http://localhost/admin
```

* Repeater me **Send** karo
* Tumhe admin panel ka HTML response mil jayega

---

### **6. Delete User ka Link dhundo**

* HTML code me search karo (`Ctrl+F`) `delete?username=carlos`
* Tumhe aisa link milega:

```
http://localhost/admin/delete?username=carlos
```

---

### **7. Final SSRF Attack bhejo**

* Ab Repeater me ek aur request bhejo jisme:

```
stockApi=http://localhost/admin/delete?username=carlos
```

* **Send** karo
* Response me confirm ho jayega ke user delete ho gaya

---

### **8. Lab Solved**

* Lab automatically solved message dega

---

💡 **Tip (PenTesting POV):**
Yeh lab tumhe sikhata hai ke SSRF kaam kaise karta hai backend ke trust ka misuse karke. Real world me aise endpoints tum internal APIs, metadata servers (AWS/GCP), ya admin panels ke liye use kar sakte ho.
