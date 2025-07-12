## 🔬 Lab: SameSite Lax Bypass via Method Override 
---

### 🧪 **Lab Goal:**

Is lab mein email change karne ka function CSRF vulnerable hai. Tera goal yeh hai ke victim ka email bina uski marzi ke change karwana (CSRF attack) — **SameSite=Lax** restriction ko bypass karke.

Tu apna khud ka account in credentials se login kar sakta hai:

```
wiener : peter
```

---

### ⚙️ **Note:**

* Victim **Google Chrome** use kar raha hai
* Chrome by default **SameSite=Lax** apply karta hai
* Tu bhi ya to Chrome ya Burp ka Chromium browser use kar

---

## 🕵️‍♂️ Step-by-Step Instructions

### ✅ Step 1: Change Email Function Samajhna

1. Burp ka browser open kar aur apne credentials se login kar
2. Email change karne ka process manually kar
3. Burp Suite ke **Proxy > HTTP history** mein jaa
4. Dekh ke `POST /my-account/change-email` request mein **koi CSRF token nahi hai**
5. `POST /login` ke response mein session cookie set ho rahi hai — lekin **SameSite attribute nahi diya**
6. Chrome is pe default **Lax** restriction laga raha hai — is ka matlab **GET requests mein cookie jayegi agar top-level navigation ho**

---

### ✅ Step 2: SameSite Restriction ko Bypass Karna

1. `POST /my-account/change-email` request ko **Repeater** mein bhej
2. Repeater mein request pe right-click kar aur "Change request method" choose kar
3. Ab Burp ne isay GET mein convert kar diya — send kar ke check kar
4. Server ne GET request reject kar di (kyunki woh POST chahta hai)
5. Ab request URL mein ye parameter add kar:

```
?_method=POST&email=foo@web-security-academy.net
```

Final request kuch aisi hogi:

```
GET /my-account/change-email?email=foo%40web-security-academy.net&_method=POST HTTP/1.1
```

6. Request send kar — agar server ne email change kar diya, to **method override kaam kar gaya**

---

### ✅ Step 3: Exploit Banana (HTML + JS)

1. Burp ke browser mein **Exploit Server** open kar
2. Body section mein yeh HTML likh:

```html
<script>
  document.location = "https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email?email=pwned@web-security-academy.net&_method=POST";
</script>
```

3. Exploit ko **store aur view** kar — check kar ke email change hua ya nahi
4. Email ko change kar de `pwned@...` mein taake victim ka alag ho
5. **Exploit victim ko deliver kar** — lab complete ho jaayega

---

## 🔚 Conclusion:

* Is lab mein tu ne dekha ke **SameSite=Lax** ka protection GET requests mein kamzor ho sakta hai
* Aur agar **method override (\_method=POST)** allow ho raha ho, to attacker GET request ko POST ki tarah treat karwa sakta hai
* Yeh technique real-world websites mein bhi kaam karti hai agar developers careless hon

