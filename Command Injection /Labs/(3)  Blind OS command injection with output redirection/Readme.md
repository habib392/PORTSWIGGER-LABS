### ⚡ Lab ka scene

* Application **feedback function** me user input (jaise email) ko OS command me directly use kar rahi hai.
* Problem ye hai ke response me output dikhaya hi nahi ja raha.
* Lekin ek **writable folder** diya gaya hai: `/var/www/images/`
* Hum command ka output isi folder me likh kar, fir us file ko website ke through access kar lenge.

---

### 🛠 Step-by-step Solution

**Step 1: Burp Suite chalao**

* Intercept karo jab tum feedback form submit karte ho.
* Request me `email` parameter hoga (ya kuch aur input).

---

**Step 2: Payload inject karo**
Normal email ki jagah yeh likho:

```
test||whoami>/var/www/images/output.txt||
```

👉 `||` ka matlab: agar pehle wali command khatam ho gayi to next command bhi chal jaaye.
👉 `whoami` command run hogi aur iska output `output.txt` file me save ho jayega jo `/var/www/images/` ke andar banegi.

---

**Step 3: Output file access karo**

* Ab product image load hone wali request intercept karo (jo images mang rahi hoti hai).
* Usme ek parameter hota hai `filename=something.jpg`.
* Uski value change karke likho:

```
filename=output.txt
```

---

**Step 4: Dekho result**
Ab tumhe response ke andar `whoami` ka output mil jaayega — jaise `www-data` ya jo bhi user process chal raha hoga.

---

### ⚡ Important Notes for Real-World PenTesting

* Yeh technique real websites pe use hoti hai jab response me command ka direct output nahi milta.
* Tab hum **output redirection (`>`, `>>`)** ya **timing-based tricks** use karte hain.
* Penetration testing me iska matlab ye hai ke tumhare paas ek tareeqa hai **blind vulnerability ko visible banane ka**.

---

Wah Habib bhai 🔥 bohat hi mast tareeqay se tumne apni soch use karke lab solve kiya hai — yeh hi asal penetration testing mindset hai 👊
Chalo isko bhi proper **step-by-step alternative method** bana dete hain jaisa tumne follow kiya:

---

### 🛠 Alternative Method

**Step 1: Feedback form submit karna**

* Website pe feedback page open kiya.
* Random info fill ki aur Burp Suite me request capture hui:

  ```
  POST /feedback/submit
  ```

---

**Step 2: Request Repeater me bhejna**

* Request ko Repeater tab me send kiya.
* `email` parameter edit karke yeh payload daala:

  ```
  test;pwd>file.txt;
  ```

👉 Yeh `pwd` command chalata hai aur output `file.txt` me likh deta hai.
👉 Response me error `"could not save"` aaya, lekin usko ignore kar diya.

---

**Step 3: File ko access karna**

* Website ke ek product image pe gaye.
* Usko **Open image in new tab** kiya.
* URL me `filename=22.txt` tha, usko replace karke likha:

  ```
  filename=file.txt
  ```
* Isse `pwd` ka output mila → `/var/www/images`.

---

**Step 4: Command change karna (whoami)**

* Dobara Burp Suite me gaye.
* Is baar `email` parameter me payload daala:

  ```
  test;whoami>file1.txt;
  ```
* Response me phir `"could not save"` error aaya, lekin ignore kar diya.

---

**Step 5: Output file dekhna**

* Fir se image request open ki aur `filename=file1.txt` set kiya.
* Output me `whoami` ka result aa gaya (jaise `www-data`).
* ✅ Lab solved.

---

### ⚡ Key Learning

* Agar direct error aata bhi hai to iska matlab yeh nahi ke command execute nahi hui — hamesha output file check karni chahiye.
* Tumne pehle `pwd` use karke directory confirm ki (bohat acha step 👍), fir `whoami` run kiya.
* Yeh ek **incremental approach** hai jo real penetration testing me safe aur reliable hoti hai.

---
