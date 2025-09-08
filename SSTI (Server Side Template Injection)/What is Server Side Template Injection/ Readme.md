### Server-Side Template Injection Kya Hai?
Yeh ek aisa attack hai jahan attacker template ke andar apna khatarnaak code daal deta hai, aur yeh code server pe chalta hai. Template engines web pages banane ke liye use hote hain, jahan fixed template aur dynamic data (jo badalta rehta hai) mix hota hai. Agar user ka input seedha template mein daala jaye, data ke roop mein nahi, toh attacker template ke syntax ka faida utha kar server ko control kar sakta hai.

---

**Misal**: Ek website pe email template hai jo user ka naam dikhata hai, jese "Dear Ali,". Agar developer user input ko carefully data ke roop mein pass karta hai, toh koi issue nahi. Lekin agar user input seedha template mein jata hai, jese "Dear " + user_ka_input, toh attacker isme apna malicious code daal sakta hai, jese `http://website.com/?name={{kharaab_code}}`.

---

### Is Se Kya Nuqsan Ho Sakta Hai?
- **Bohot bara nuqsan**: Attacker server ka poora control le sakta hai (Remote Code Execution), matlab server ko apne hisaab se chala sakta hai, sensitive data chura sakta hai, ya internal systems pe attack kar sakta hai.
- **Chhota nuqsan**: Agar poora control na bhi mile, toh bhi attacker sensitive files ya data padh sakta hai.

---

### Yeh Vulnerability Kyun Hoti Hai?
Jab developer user input ko seedha template mein daal deta hai, bina data ke roop mein pass kiye. **Misal**: Ek email template mein user apna naam choose karta hai, aur wo naam seedha template mein jata hai, jese:

```php
$output = $twig->render("Dear " . $_GET['name']);
```

Yahan, agar attacker `name` parameter mein khatarnaak template code daal de, toh server usko execute kar dega. Yeh galti aksar developers ke ghalat design ya security knowledge ki kami ki wajah se hoti hai. Kabhi kabhi, websites jaan boojh kar kuch users (jese content editors) ko templates edit karne ki permission dete hain, jo bhi risky hai.

---

### Attack Kaise Plan Kiya Jata Hai?
1. **Pehchan**: Pehle yeh check karo ke website template use kar rahi hai ya nahi. User input template mein directly jata hai ya data ke roop mein?
2. **Test**: Template ke syntax ke saath experiment karo, jese `{{7*7}}` daal kar dekho ke result `49` aata hai ya nahi. Agar aata hai, toh vulnerability hai.
3. **Exploit**: Ab malicious payload banao jo template engine ke hisaab se server pe execute ho sake, jese server ka control lene ke liye ya data churana.

---

### Kaise Bachao?
- User input ko hamesha **data** ke roop mein pass karo, na ke template mein directly.
- Template syntax ko sanitize karo, taake khatarnaak code execute na ho.
- Agar users ko templates edit karne ki permission hai, toh strict access controls lagao.

---

### **Attack Kaise Plan Kiya Jata Hai?**
Yeh ek step-by-step process hai:

#### **1. Detect (Pehchan)**
- Pehle check karo ke website template use kar rahi hai ya nahi.
- Special characters ya template syntax try karo, jese `${{<%[%'"}}%\`.
- Agar server error deta hai ya output badalta hai, toh shayad vulnerability hai.

**Misaal**: Ek URL try karo: `http://website.com/?username=${7*7}`. Agar output mein "49" aata hai, toh server template syntax evaluate kar raha hai, yani vulnerability hai.

- Do contexts hote hain:
  - **Plaintext Context**: Jahan user input seedha template mein jata hai, jese `render('Hello ' + username)`. Isme `username=${7*7}` try karo. Agar "Hello 49" aata hai, toh vulnerability confirm hai.
  - **Code Context**: Jahan user input template expression ke andar jata hai, jese `engine.render("Hello {{"+greeting+"}}", data)`. Isme URL aisi try karo: `http://website.com/?greeting=data.username}}<tag>`. Agar output mein HTML tag render hota hai, toh vulnerability hai.

#### **2. Identify (Template Engine Pehchano)**
- Har template engine ka syntax alag hota hai. Invalid syntax try karo, jese `<%=foobar%>`. Error message se pata chal sakta hai ke kaunsa engine hai (jese Ruby ka ERB).
- Mathematical operations test karo, jese `{{7*'7'}}`. Twig mein yeh 49 dega, lekin Jinja2 mein 7777777. Isse engine confirm karo.

**Misaal**: Agar `{{7*7}}` se output 49 aata hai, toh shayad Twig engine hai. Agar error aata hai ya alag output, toh doosra engine ho sakta hai.

#### **3. Exploit (Attack Karo)**
- Jab engine pata chal jaye, toh uske hisaab se malicious payload banao. Jese, server pe code execute karne ke liye ya sensitive data churana.
- Har engine ke liye alag payloads hote hain, toh research karo ke kaunsa kaam karega.

**Misaal**: Agar Twig engine hai, toh payload jese `{{_self.env.registerUndefinedFilterCallback("exec")}}` try karke server pe command chala sakte ho.

---

### **Kaise Bachao?**
1. **User Input ko Data ke Roop Mein Pass Karo**: Hamesha user input ko template ke andar data ke roop mein daalo, na ke seedha template mein.
   - **Safe**: `$twig->render("Dear {first_name},", array("first_name" => $user.first_name));`
   - **Unsafe**: `$twig->render("Dear " . $_GET['name']);`

2. **Logic-Less Template Engines Use Karo**: Jese Mustache, jo logic aur presentation ko alag rakhta hai, taake attack ka chance kam ho.

3. **Sandbox Environment**: Agar users ko templates edit karne ki permission deni hai, toh unka code sandbox mein chalaao, jahan dangerous functions disabled hon.

4. **Docker Container**: Template environment ko locked-down Docker container mein chalaao, taake agar code execute bhi ho, toh nuqsan limited rahe.

**Misaal**: Ek e-commerce website ne user input ko seedha template mein daal diya. Hackers ne iska faida utha kar customer data chura liya. Agar unhone Mustache use kiya hota ya sandboxing ki hoti, toh yeh attack roka ja sakta tha.

---

### **Aur Kya Kar Sakta Hai?**
- **Labs Try Karo**: Web Security Academy ke labs mein realistic scenarios pe practice karo. Yeh deliberately vulnerable targets hote hain jo tujhe skills improve karne mein madad dete hain.
- **Research Padho**: PortSwigger ne 2015 mein ispe research ki thi, jisme live websites pe yeh vulnerabilities exploit ki gayi. Unki research padh sakta hai for more details.
