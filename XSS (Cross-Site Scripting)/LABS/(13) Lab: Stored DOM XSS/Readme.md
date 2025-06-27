# Lab: Stored DOM XSS

### 🎯 Goal:
Comment section ka use karke alert(1) execute karwana — XSS payload DOM mein store ho raha hai aur render hote waqt trigger ho raha hai.

### 🛠️ Tools Required:
- Browser
- (Optional) Burp Suite for observation — lekin is lab mein directly browser se ho jaata hai

### 🚶 Step-by-Step Solution:
🔹 Step 1: Lab ko Open Karo

Lab ka interface open hoga jahan blog aur comment section dikh raha hoga.

🔹 Step 2: Comment Section Dhoondo

Blog post ke neeche comment box hoga jahan tu koi comment daal sakta hai (name + comment).

🔹 Step 3: XSS Payload Socho

Lab ke description se pata chalta hai ke:

Site ne replace(```"<", "&lt;"```) jaisa koi filter lagaya hai — lekin sirf pehla angle bracket replace hota hai.

Toh iska matlab:

Agar tu ```<img src=1 onerror=alert(1)>``` daalta hai to first < encode ho jaata hai

Lekin agar tu 2 dafa < daalein, to doosra pass ho jaata hai!

🔹 Step 4: Final Payload Choose Karo

Tu do options use kar sakta hai:

✅ Simple bypass payload (lab ka official solution):

```<><img src=1 onerror=alert(1)>```

✅ Tera working payload (bhi correct hai):

```<script><img src=x onerror=alert(1)></script>```

Dono kaam karte hain kyunki filtering weak hai.

🔹 Step 5: Comment Submit Karo
Apna naam likho (e.g., Habib)

Comment box mein payload paste karo

Submit dabao

🔹 Step 6: Page Reload Hoga — Alert Popup Aayega ✅
Jaise hi page reload hoga, comment render hoga DOM mein, aur XSS trigger hoke alert(1) dikhega.

### LAB SOLVED

---

| Point       | Detail                                                              |
| ----------- | ------------------------------------------------------------------- |
| **Type**    | Stored DOM-Based XSS                                                |
| **Source**  | Comment form (user input)                                           |
| **Sink**    | DOM manipulation (`innerHTML` ya similar)                           |
| **Filter**  | `replace("<", "&lt;")` only once                                    |
| **Bypass**  | First `<` ko encode hone do, doosra payload chala lo                |
| **Payload** | `<><img src=1 onerror=alert(1)>`                                    |
| **Result**  | Payload DOM mein render hua → script execute → `alert(1)` triggered |

---

### 🧠 Key Lesson:
Agar filter sirf ek hi dafa replace kar raha ho, toh multiple angle brackets ya payload shifts se tu usay bypass kar sakta hai.
JavaScript ke replace() method ko agar global (/g) na banaya ho, toh yeh bug mil sakti hai.

