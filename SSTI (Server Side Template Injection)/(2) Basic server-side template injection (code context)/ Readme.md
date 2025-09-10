Server-Side Template Injection (SSTI) ka matlab hai developer ne user input ko directly template engine main inject kar diya bina sanitize kiye. Tornado ek Python based template engine hai. Agar hum isko manipulate karen, to Python code run ho sakta hai server pe.

---

### A to Z Solution for This Lab

1. **Login karna**
   Credentials diye gaye hain:

   ```
   username: wiener
   password: peter
   ```

   Login kar lo.

2. **Comment add karna**
   Kisi bhi blog post pe comment kar do. Yeh isliye zaroori hai taake author name display ka system test kar saken.

3. **Account setting check karna**
   "My account" page pe option hota hai ki tumhara comment kis name se show ho — `Full name`, `First name`, ya `Nickname`.
   Jab tum option select karte ho, ek **POST request** jaati hai:

   ```
   POST /my-account/change-blog-post-author-display
   blog-post-author-display=user.name
   ```

4. **Burp Proxy/Reapeater main request capture karna**
   Yeh request Repeater bhej do aur payload test karna shuru karo.

5. **Injection test karna**
   Tornado documentation se pata chalta hai ke variables `{{ }}` curly braces main likhe jaate hain.
   Example payload:

   ```
   blog-post-author-display=user.name}}{{7*7}}
   ```

   Agar tumhare comment ke upar "49" show ho gaya to matlab SSTI confirmed hai ✅

6. **Arbitrary Python code execute karna**
   Tornado templates allow karte hain Python statements ko `{% %}` ke andar.
   Example:

   ```
   {% import os %}
   ```

   Isse hum Python ka `os` module use kar sakte hain.

7. **Command execute karna**
   `os.system('command')` ke zariye system commands run hote hain.
   Hume delete karna hai:

   ```
   rm /home/carlos/morale.txt
   ```

8. **Final payload banana**
   Payload kuch aisa hoga:

   ```
   blog-post-author-display=user.name}}{% import os %}{{os.system('rm /home/carlos/morale.txt')}}
   ```

   Lekin request main yeh URL-encoded bhejna hoga:

   ```
   blog-post-author-display=user.name}}{%25+import+os+%25}{{os.system('rm%20/home/carlos/morale.txt')}}
   ```

9. **Request bhejna**
   Burp Repeater se yeh request submit karo.

10. **Comment page reload karna**
    Wapas blog page load karo jahan tumhara comment tha.
    Jaise hi template render hoga, tumhara payload execute hoga aur `morale.txt` delete ho jaayegi ✅
    Lab solved!

---

⚡ **Real-world penetration testing tip:**
SSTI ka faida ye hai ke attacker directly server-side code execute kar sakta hai. Yeh Remote Code Execution (RCE) jaisa powerful vulnerability hai. Agar tumko kabhi live bug bounty ya pentest main milay, to isko responsibly report karna aur batana ke arbitrary OS commands run karna possible hai.

