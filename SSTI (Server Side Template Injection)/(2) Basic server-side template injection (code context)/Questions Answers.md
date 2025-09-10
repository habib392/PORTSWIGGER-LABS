### Command Breakdown

```
blog-post-author-display=user.name}}{%25+import+os+%25}{{os.system('rm%20/home/carlos/morale.txt')}}
```

1. **`blog-post-author-display=`**
   Yeh parameter ka naam hai jo POST request main ja raha hai. Iski value badal kar hum injection karte hain.

2. **`user.name`**
   Yeh normally variable hai jo user ka name show karta hai template main. Default value yehi hoti hai.

3. **`}}`**
   Yeh Tornado ka **template closing braces** hain. Matlab humne `user.name` wali expression ko close kar diya.
   (Ab server samjhega ke ye variable khatam ho gaya).

4. **`{%25+import+os+%25}`**

   * `{% ... %}` Tornado main Python code likhne ka syntax hai.
   * `%25` = URL encoding of `%`.
   * Matlab jab server decode karega to yeh actually banega:

     ```
     {% import os %}
     ```
   * Yeh Python ka `os` module import karta hai, jo system commands run karne ke liye chahiye.

5. **`{{os.system('rm%20/home/carlos/morale.txt')}}`**

   * `{{ ... }}` Tornado main expression evaluate aur print karne ke liye hota hai.
   * `os.system(...)` Python ka function hai jo directly system shell commands run karta hai.
   * `'rm%20/home/carlos/morale.txt'`

     * `rm` = Linux command jo file delete karti hai.
     * `%20` = space (URL encoded).
     * `/home/carlos/morale.txt` = target file jo hume delete karni hai.

6. **Last `}}`**
   Yeh expression close kar raha hai.

---

### Simple Flow

1. `user.name` close kiya → `}}`
2. Python code inject kiya → `{% import os %}`
3. System command run karaya → `{{os.system('rm /home/carlos/morale.txt')}}`

---

⚡ Matlab humne **template ka original context tod kar** apna Python + system command inject kar diya.
Yehi hai **Server-Side Template Injection → Remote Code Execution** ka asli magic.

---

## Questions 
Yeh single curly brackets {} or double curly brackets {{}} inn main kya farq hai??

---

## Answer

### Tornado (aur zyada templates) main difference:

1. **Single curly `{ }`**
   Normally koi special kaam nahi karti. Agar tum template main sirf `{ }` likh do to wo as a **normal text** treat hoti hai.
   Example:

   ```
   Hello {world}
   ```

   Page par simply dikh jaayega:

   ```
   Hello {world}
   ```

---

2. **Double curly `{{ ... }}`**
   Ye **expression evaluate aur output** karta hai.
   Matlab jo andar likha hoga wo run hoga aur uska result print ho jaayega.

   Example:

   ```
   {{7*7}}
   ```

   Output:

   ```
   49
   ```

   Agar tum likho:

   ```
   {{user.name}}
   ```

   To page par user ka actual naam aa jaayega.

---

3. **`{% ... %}`** (ye bhi related hai)
   Ye **statements ya logic** ke liye hota hai (loops, imports, condition, etc.).
   Example:

   ```
   {% import os %}
   ```

   Matlab Python ka `os` module load ho gaya.

---

### Short Summary:

* `{ }` → normal text
* `{{ ... }}` → expression evaluate & print result
* `{% ... %}` → logic / Python code execution

---

⚡ Real-world penetration testing tip:
Jab bhi tumhe template injection ka doubt ho, pehla test yahi hota hai:

* `{{7*7}}` → agar output me `49` aata hai → SSTI confirmed ✅

---
