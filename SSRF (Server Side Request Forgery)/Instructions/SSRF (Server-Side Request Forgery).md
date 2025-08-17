**Server-Side Request Forgery (SSRF) kya hai?**

SSRF ek web security ka masla hai jahan attacker server ko aisa request banane pe majboor karta hai jo uski marzi ke mukhtalif jagahon pe jata hai. Simple zuban mein, yeh ek tarah ka attack hai jismein attacker server ko bolta hai ke woh kisi aisi jagah connect kare jahan usay nahi jana chahiye.

### **SSRF kya hota hai?**
SSRF mein attacker server-side application ko manipulate karta hai taake woh galat jagah, jaise internal systems ya external websites, se connect kare. Maslan, ek attacker server ko organization ke internal services (jo sirf andar se accessible hote hain) ya kisi external system se data mangne ke liye use kar sakta hai. Isse sensitive data, jaise login credentials, leak ho sakte hain.

### **SSRF attacks ka asar kya hai?**
- **Unauthorized access**: Attacker organization ke sensitive data ya systems tak pohonch sak encountering data leak ya unauthorized actions kar sakta hai.
- **Command execution**: Kabhi kabhi, SSRF se attacker server pe commands chala sakta hai.
- **Malicious attacks**: Agar server external systems se connect karta hai, toh attacker aisa attack launch kar sakta hai jo organization se aata hua lage.

### **Common SSRF attacks**
SSRF attacks aksar trust relationships ko exploit karte hain, yaani server ya back-end systems ke beech ke bharose ko galat istemal karte hain.

#### **1. Server ke khilaf SSRF attack**
Yahan attacker server ko usi ke loopback interface (jaise `127.0.0.1` ya `localhost`) pe request bhejne ke liye majboor karta hai. Maslan, ek shopping app mein user item ka stock check karta hai, aur server ek back-end API se data lata hai. Request aisi hoti hai:

```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1
```

Agar attacker is request ko modify karke aisa kare:

```
stockApi=http://localhost/admin
```

Toh server `/admin` URL se data fetch karega, jo normally sirf trusted users ke liye hota hai. Kyunki yeh request server ke apne local system se aata hai, access controls bypass ho jate hain, aur attacker ko sensitive admin features mil sakte hain.

**Kyun hota hai aisa?**
- Access controls server ke samne wale component mein hote hain, jo local requests ko check nahi karte.
- Disaster recovery ke liye, server local requests ko trusted manta hai.
- Admin interface alag port pe hota hai, jo users ke liye directly accessible nahi hota.

#### **2. Back-end systems ke khilaf SSRF**
Kabhi server aise internal systems se connect karta hai jo users ke liye nahi hote, jaise private IP addresses (e.g., `192.168.0.68`). Yeh systems aksar kam secure hote hain. Attacker aisa request bhej sakta hai:

```
stockApi=http://192.168.0.68/admin
```

Isse attacker internal admin interface tak pohonch sakta hai bina authentication ke.

### **SSRF defenses ko kaise bypass karte hain?**
Applications mein SSRF ko rokne ke liye filters hote hain, lekin inhe bypass kiya ja sakta hai.

#### **1. Blacklist-based filters**
Agar app `127.0.0.1` ya `localhost` ko block karti hai, toh attacker in tarikon se filter bypass kar sakta hai:
- Alternate IP formats, jaise `2130706433`, `127.1`, ya `017700000001`.
- Apna domain banaye jo `127.0.0.1` resolve kare, maslan `spoofed.burpcollaborator.net`.
- URL encoding ya case variation use karein, jaise `lOcAlHoSt`.
- Ek controlled URL jo redirect kare, maslan `http://attacker.com` jo `http://localhost/admin` pe redirect kare.

#### **2. Whitelist-based filters**
Agar app sirf specific URLs allow karti hai, toh yeh tricks kaam aa sakti hain:
- Credentials embed karein: `https://expected-host:fakepassword@evil-host`.
- URL fragment use karein: `https://evil-host#expected-host`.
- DNS hierarchy ka faida uthayein: `https://expected-host.evil-host`.
- URL encoding ya double encoding use karein taake filter confuse ho.

#### **3. Open redirection se bypass**
Agar app mein open redirection vulnerability hai, toh attacker isay SSRF ke liye use kar sakta hai. Maslan:

```
stockApi=http://weliketoshop.net/product/nextProduct?currentProductId=6&path=http://192.168.0.68/admin
```

Yeh URL pehle allowed domain pe jati hai, phir redirect se internal system tak pohonch jati hai.

### **Real-world example**
Ek online store ke app mein, jab tum stock check karte ho, server back-end se data mangta hai. Agar attacker URL ko `localhost/admin` ya internal IP jaise `192.168.0.68/admin` pe point kare, toh woh sensitive admin panel tak pohonch sakta hai. Yeh kaafi khatarnak hai kyunki internal systems aksar kam secure hote hain.

### **Kaise rokein?**
- **Strict validation**: URLs ko carefully check karein aur sirf trusted domains allow karein.
- **Network-level protection**: Internal systems ko private IPs pe isolate karein.
- **Disable redirects**: Agar zarurat na ho, toh server ko redirects follow karne se rokein.
- **Monitor requests**: Server ke outgoing requests ko track karein.
