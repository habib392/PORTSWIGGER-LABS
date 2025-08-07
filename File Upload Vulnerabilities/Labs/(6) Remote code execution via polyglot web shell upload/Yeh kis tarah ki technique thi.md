
## 🧠 Lab Analysis: Remote Code Execution via Polyglot Web Shell Upload

---

### 📌 **Yeh kis tarah ki technique thi?**

Yeh technique **Polyglot File Upload Attack** kehlati hai.
Iska matlab hai ek aisi file upload karna jo **dikhne mein image ho**, lekin **asliyat mein executable code ho** — jaise PHP ka code.

Is technique mein hum **image ke metadata** (jaise "Comment" field) mein malicious PHP code chhupa dete hain.
File ko `.php` extension dete hain, takay server jab isse serve kare, to code execute ho jaye.

---

### 💡 **Iss lab mein kya khaas baat thi?**

1. **Server file upload par image validation kar raha tha**, lekin sirf **surface level** — yaani content-type ya image header dekh raha tha.
2. Server ne `.php` file extension allow kar di **kyunki image ki tarah lag rahi thi** (metadata ke wajah se).
3. **PHP code file ke andar directly nahi tha**, balkay **image ke comment metadata** mein tha.
4. Jab user ne image ko visit kiya, PHP server ne uss comment ko process kar liya aur malicious code run ho gaya.

---

### 📍 **Iss lab ke main points kya thay?**

| 🔢 | Point                                                                | Detail                                                    |
| -- | -------------------------------------------------------------------- | --------------------------------------------------------- |
| 1  | File extension `.php` hone ke bawajood server ne image accept kar li | Kyunki file image ki tarah behave kar rahi thi (polyglot) |
| 2  | Metadata mein PHP payload chhupa tha                                 | Comment field ke zariye                                   |
| 3  | Server ne deep validation nahi kiya                                  | Sirf upar upar se file ko image samjha                    |
| 4  | PHP code execute ho gaya jab image URL access hua                    | Isse attacker ko RCE mil gaya                             |
| 5  | Burp Suite se response analyze karke secret key mili                 | `START ... END` ke beech                                  |

---

### 🤔 **Kya aaj bhi aisi vulnerability milti hai?**

**Haan**, aaj bhi milti hai — lekin mostly **small companies** ya **poorly coded CMS** systems mein:

* WordPress plugins
* Custom file upload scripts
* PHP-based old systems

Mostly yeh vulnerabilities milti hain jahan developer:

* Sirf file extension check karta hai (e.g. `.jpg`)
* Ya sirf Content-Type header check karta hai (e.g. `image/jpeg`)
* Lekin **actual file parsing** nahi karta image library se (e.g. GD, ImageMagick)

---

### 👨‍💻 **Kya yeh developer ki ghalti hoti hai?**

**Bilkul.** Yeh 100% developer ki security misconfiguration hoti hai.

Developer ko chahiye:

* File ka naam ya extension par depend na kare.
* File ko proper image parsing library se parse kare.
* File size, dimensions, type, aur EXIF metadata bhi sanitize kare.
* File ko server ke executable folder mein **kabhi bhi** store na kare.

Yeh vulnerability hoti hai jab developer **secure file handling** aur **validation** properly nahi karta.

---

### 📢 **Real World Advice (Pentesting View)**

Jab bhi koi **image upload** feature mile, toh yeh try karo:

* `.php`, `.phar`, `.phtml` extension ke saath polyglot file upload karo
* `ExifTool` se metadata injection test karo
* Burp Suite se server ka behavior analyze karo
* Uploaded image ka **direct URL** access karke code execution observe karo

