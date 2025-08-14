### **Yeh Lab Kis Tarah Ki Technique Thi?**

Yeh ek **SSRF** (Server-Side Request Forgery) attack tha.
SSRF ka matlab simple shabdon main: tum server ko force karte ho ke woh tumhari marzi ka request bheje — chahe woh internal network pe ho ya koi hidden service pe.

Iss lab main humne website ke "Check Stock" feature ka misuse karke **internal IP range scan ki** aur admin panel find kiya, phir ek request bhej ke Carlos ko delete kar diya.

---

### **Iss Lab Main Khaas Baat**

1. **Internal Network Access:**
   Humne directly internal IPs (192.168.0.X) ko hit kiya jo normally outside world se access nahi hoti.

2. **Port Specific Targeting:**
   Humein pata tha ke admin panel port **8080** pe hai, toh humne specifically iss port ko test kiya.

3. **Automated Scanning via Intruder:**
   Burp Intruder se ek hi request ka IP address ka last octet change karke pura network scan kar liya.

4. **Direct Action Execution:**
   Admin panel milte hi humne URL parameter change karke action execute kar di (`/delete?username=carlos`).

---

### **Main Points**

* Vulnerable parameter: `stockApi`
* Target network: `192.168.0.X`
* Target port: `8080`
* Method: Internal network scan → Admin interface find → Action perform
* Tools used: Burp Suite (Proxy, Intruder, Repeater)

---

### **Kya Yeh Vulnerability Aaj Bhi Milti Hai?**

Haan, SSRF aaj bhi real-world apps main milti hai, especially:

* Cloud servers (AWS, Azure, GCP)
* Misconfigured APIs
* Old internal admin panels
* PDF generators, image fetchers, webhook handlers

---

### **Yeh Vulnerability Developer Ki Wajah Se Hoti Hai?**

Haan, 100%.
Developer ki galtiyan:

* **User input ko validate na karna** (e.g., `stockApi` ka URL directly use kar liya bina filter kiye)
* **Internal network ko expose kar dena**
* **Allow list / deny list na lagana** for outbound requests
* **SSRF protection** jaise URL parsing, IP range blocking na karna

---

### **Penetration Testing Angle**

* Jab bhi tum web app test karo, aise parameters dhoondo jo **URL accept karte hain**.
* Unko change karke internal network, metadata endpoints, ya sensitive APIs hit karne ki koshish karo.
* Cloud main yeh technique bohot dangerous ho sakti hai kyunki tum credentials, secrets, aur config le sakte ho.

---

**Example:**

```http
stockApi=http://192.168.0.54:8080/admin/delete?username=carlos
```

Yeh ek real SSRF payload ka example hai jo lab solve karta hai.
