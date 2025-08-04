### 📂 Path Traversal kya hota hai?

**Path Traversal** (yaani **Directory Traversal**) ek aisi vulnerability hai jo attacker ko server ke andar ke **important files** ko access karne ka moka deti hai — jaisy configuration files, credentials, ya even system files.

---

### ⚙️ Kaise kaam karta hai?

Misaal ke tor pe soch ke aik shopping website hai jo har product ki image dikhati hai:

```html
<img src="/loadImage?filename=218.png">
```

Yahan `/loadImage` ek backend function hai jo `filename=218.png` ko uthata hai aur image dikhata hai. Server ye image yahan sy read karta hai:

```
/var/www/images/218.png
```

Lekin agar koi attacker URL mein file ka naam badal de:

```
/loadImage?filename=../../../etc/passwd
```

To ab yeh file ban jaati hai:

```
/var/www/images/../../../etc/passwd
```

`../` ka matlab hota hai “ek folder peeche jao”. To:

* `../../../etc/passwd` ka matlab hua:
  pehle `/var/www/images/` se bahar niklo → phir root `/` directory mein jao → phir `etc/` folder → aur `passwd` file uthao.

---

### 🧠 Yeh attacker ko kya faida deta hai?

* **Linux pe**: `/etc/passwd` file mein sab users ke naam hotay hain.
* **Windows pe**: wo `..\..\..\windows\win.ini` jesi file access kar sakta hai.

Is tarah attacker server ke secret data tak pahunch sakta hai — aur agar write karne ki permission bhi mil gayi to server ka **control le sakta hai**.

---

### 🛡️ Isse bachao kaise?

1. **User input sanitize karo** — `../` jesi cheezen block karo.
2. **Allow list banao** — sirf safe filenames allow karo.
3. **Path normalize karo** — file ka asal path calculate karke check karo ke allowed directory ke andar hi ho.
4. **Web server permissions theek rakho** — server ko unnecessary files access karne ki permission na do.

---

### 💡 Real World Use (Penetration Testing Tip):

Jab koi web app file download ya image load karti ho aur URL mein filename dikh raha ho (`?file=...`), to yeh **path traversal** ka potential hota hai. Tum burp suite se `../` inject karke test kar sakte ho ke koi sensitive file access hoti hai ya nahi.

Chalo Habib, ab main is second part ko bhi teri zuban mein asaan aur practical andaaz mein samjhaata hoon — pehle humne path traversal kya hoti hai seekha, ab **usko exploit karne mein kya rukawat aati hai** aur **kaise bachao kiya jaata hai**, wo dekhtay hain.

---

## 🧱 **Path Traversal Attack mein rukawatein kya hoti hain? (Obstacles)**

Kuch applications try karti hain ke `../` jaise directory traversal sequences ko **block ya remove** kar dein. Lekin hum attacker ho ke phir bhi unko bypass karne ki **tricks** laga sakte hain:

---

### 💡 **Bypass Techniques**

1. ### ✅ **Absolute Path se direct file access**

   Agar server allow karta hai, to `filename=/etc/passwd` jaise full path deke direct file read kar sakte ho — bina `../` ke.

2. ### 🌀 **Nested Traversal Sequences**

   Kabhi kabhi app `../` ko remove karti hai, to hum aise likh sakte hain:

   * `....//etc/passwd`
   * `....\/etc/passwd`
     Ye clean hone ke baad `../etc/passwd` ban jaata hai.

3. ### 🧩 **URL Encoding / Double Encoding**

   * `../` ko `%2e%2e%2f` banao
   * Double encode karo to: `%252e%252e%252f`
   * Kuch aur tricks: `..%c0%af` ya `..%ef%bc%8f`
     Har server inko alag tarike se decode karta hai.

4. ### 🏠 **Base Folder ka Rule Bypass Karna**

   Agar app chahti hai ke file `/var/www/images/` se hi start ho, to:

   ```
   /var/www/images/../../../etc/passwd
   ```

   likh kar base folder ki shakal bana lo, lekin us se bahar nikal jao.

5. ### 🎭 **Fake Extension + Null Byte Attack**

   Agar app chahti hai `.png` end mein ho, to:

   ```
   ../../../etc/passwd%00.png
   ```

   `%00` (null byte) se backend `.png` ignore kar deta hai. (Modern languages mein kaam nahi karta zyada, lekin kabhi kabhi purani PHP versions mein chalta hai.)

---

### 🧰 **Burp Suite ka Use (Pro Tip)**

Burp Intruder mein ek built-in payload list hoti hai:
**`Fuzzing - path traversal`**
Is list mein **encoded traversal sequences** hoti hain jinhe tum automatic try karwa sakte ho.

---

## 🔐 **Path Traversal se bachao kaise karein? (Defense)**

### ✅ Sabse safe tareeqa:

**User input ko file system APIs tak pohanchane hi mat do.**
Yani user se filename lene ki bajaye khud hardcoded file choose karo.

---

### Agar lena zaroori ho to 2 layered defense:

1. **Input Validation:**

   * **Whitelist:** sirf `218.png`, `219.png` jaisy files allow karo.
   * Agar list na ho to sirf alphabets/numbers allow karo (`^[a-zA-Z0-9_.-]+$`)

2. **Canonical Path Check (Java Example):**

```java
File file = new File(BASE_DIRECTORY, userInput);
if (file.getCanonicalPath().startsWith(BASE_DIRECTORY)) {
    // process file
}
```

Matlab:

* Pura path resolve karo (`canonical path`)
* Check karo ke wo expected directory ke andar hi ho

---

### 🧠 Real-World Pentesting Tip:

Jab tum kisi app mein `?file=` ya `?image=` jaise parameters dekho, ya koi file uploader ho jahan `multipart/form-data` aata ho, to **URL encoding, nested traversal, or base folder bypass** try karo. Yeh har burp repeater ya intruder se test kiya ja sakta hai.

---

