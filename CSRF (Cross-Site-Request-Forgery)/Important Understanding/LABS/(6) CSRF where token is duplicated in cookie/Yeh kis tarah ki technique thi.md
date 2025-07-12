CSRF token cookie aur request dono mein hona — "Double Submit" technique

Kuch websites CSRF token ko server side pe store nahi karti. Balkay wo har request mein token ko 2 jagah check karti hain:

1. Request body mein (jaise form ke andar)

2. Cookie mein (jo browser ke saath chali aati hai)

Phir server bas itna check karta hai:

> "Kya dono jagah ka token same hai?"

Agar haan, toh wo request ko valid maan leta hai.
Isay “Double Submit Cookie Defense” kehte hain.


---

❗ Ismein attacker kya kar sakta hai?

Attacker:

Apna khud ka token generate karta hai (chahe wo fake hi kyun na ho)

Apni site (ya kisi subdomain) ke zariye victim ke browser mein ek cookie set karta hai jismein wo token hota hai

Phir CSRF attack ke zariye victim se wahi token waali request bhejta hai


Server yeh sochta hai ke “yeh token valid hai, dono jagah same hai”, lekin asal mein attacker ne hi dono jagah ka token set kiya hota hai.


---

🛠️ Real-World PenTesting Tip:

Jab bhi double submit token mechanism dekho, toh check karo:

Kya attacker cookie set kar sakta hai? (subdomain se ya koi open endpoint se)

Kya token validate ho bhi raha hai ya sirf match check ho raha hai?

---

## 🔍 HTML Exploit Ka Deep Analysis:

| Line                               | Explanation                                                                        |
| ---------------------------------- | ---------------------------------------------------------------------------------- |
| `<form ...>`                       | Victim ke browser se email change request bhejta hai                               |
| `csrf=fake`                        | Token jo cookie se match karta hai                                                 |
| `<img src=... onerror=...>`        | Jab image load nahi hoti, `onerror` trigger hota hai jo form submit karta hai      |
| `/search=test%0d%0aSet-Cookie:...` | Cookie injection exploit hai — search endpoint se custom header inject ho raha hai |

---

## 🛡️ Important Notes:

### ❓ Agar Victim logged-in nahi ho to?

➡️ **CSRF kaam nahi karega** — kyunki browser ke paas session cookie nahi hogi
➡️ Server request reject kar dega

---

### ❓ Agar victim Facebook pe login hai, aur tum usko PortSwigger exploit bhejo?

➡️ **Facebook pe koi asar nahi hoga**
➡️ CSRF sirf **usi domain pe kaam karta hai** jiska `action` diya gaya hai

---

## 🔥 Real-World CSRF Uses:

| Use-case                           | Impact                   |
| ---------------------------------- | ------------------------ |
| Email change                       | Account hijack           |
| Password change (no old pwd check) | Full takeover            |
| Bank transaction                   | Money loss               |
| Shipping address change            | Parcel theft             |
| Admin add in CMS                   | Website ka control milna |

---

## ⚔️ Q\&A Summary:

### Q: Agar victim ka account hi na ho us site pe?

➡️ CSRF kaam nahi karega — kyunki login/session hi nahi hai

---

### Q: Kya attacker ko victim ka purana email mil jata hai?

❌ Nahi milta — server disclose nahi karta
✔️ Lekin guess kar ke reset attempt karo to confirm ho sakta hai (error ya success response se)

---

### 🧠 CSRF Ka Real Faida Tabhi Hai Jab:

* Website vulnerable ho
* Victim login ho
* Sensitive action bina validation ke perform ho jaye


