# 🧪 Lab Title:

Reflected XSS with AngularJS sandbox escape and CSP

---

🧠 Samajhne Wali Cheez:

Is lab mein teen barriers hain:

1. ✅ Reflected XSS = Input URL mein daalo aur browser ke response mein render ho

2. ✅ AngularJS Sandbox = Dangerous code detect karke rokta hai

3. ✅ CSP (Content Security Policy) = Dangerous sources, inline JS, window, alert() waghaira block karta hai

Humein CSP + Sandbox + XSS teeno ko bypass karke alert(document.cookie) chalaana hai. Bohot hi maze ka challenge hai!

---

✅ Step-by-Step A to Z Tareeqa

🥇 Step 1: Lab Open Karo

Lab open karo aur sidebar mein Exploit Server button dikh raha hoga — uspe click karo


---

🥈 Step 2: Yeh Payload Paste Karo

<script>
location='https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cinput%20id=x%20ng-focus=$event.composedPath()|orderBy:%27(z=alert)(document.cookie)%27%3E#x';
</script>

👉 YOUR-LAB-ID ko apne actual lab domain se replace karo (jo URL mein likha hota hai)


---

🥉 Step 3: Store & Deliver

Exploit Server pe:

Click Store

Phir click Deliver exploit to victim




---

🔍 Kaise kaam karta hai yeh Payload?

Chal isko real life story se samjhaata hoon:


---

🎭 Real Life Analogy:

Socho ek school hai jahan headmaster (CSP) kehti hai:

> "Koi student direct mere office (window object) mein nahi aa sakta!"



Student (XSS attacker) chalaak hai. Wo kya karta hai:

1. Ek form banata hai (input element) jisme likha hota hai:
"ng-focus=$event.composedPath() | orderBy: '(z=alert)(document.cookie)'"


2. Jaise hi koi focus karega, AngularJS ka event fire hoga:

$event.composedPath() return karega path jisme sab elements list honge

Last mein hamesha window hota hai



3. orderBy AngularJS filter hai — jo array elements pe logic chalaata hai

(z=alert) matlab humne alert ko ek variable z mein store kiya

(z)(document.cookie) ab yehi alert function cookie ko alert karega



4. Kyunki humne window.alert() directly nahi likha, sandbox aur CSP confuse ho gaye



💥 Result: alert(document.cookie) chal gaya!


---

📚 Important Terms Samajh Le:

| Term                   | Kya hai (Simple Explanation)                                        |
| ---------------------- | ------------------------------------------------------------------- |
| `ng-focus`             | AngularJS directive – jab input pe focus aaye to ye chalega         |
| `$event`               | Wo object jisme event details hoti hain                             |
| `composedPath()`       | Ek array deta hai jisme event ka full path hota hai (last = window) |
| `orderBy:`             | AngularJS ka filter jisme hum expression run karwa sakte hain       |
| `(z=alert)`            | `alert` function ko z variable mein store kiya                      |
| `(z)(document.cookie)` | z ko function samajh ke call kiya with document.cookie              |

---

✅ Jab Exploit Victim Tak Pohonchta Hai

Victim (browser):

Input field detect karta hai

ng-focus trigger hota hai

AngularJS sandbox confused hota hai

CSP samajhta hai koi dangerous kaam nahi ho raha

alert(document.cookie) chal jaata hai

Lab Solved! 🎉



---

🔐 Extra Security Insight (PenTest Tip 💡)

Agar kisi site mein:

AngularJS ka use ho raha ho (check via console: angular.version.full)

Input fields mein ng- wale attributes hon

CSP headers ho (Inspect > Network > Headers tab)


Toh aisi site XSS + CSP bypass ka strong candidate hoti hai!


---

🧾 Final Summary (Add to GitHub Note)

> Jab hum kisi website ka input field dekhte hain jo AngularJS use karta ho aur us pe CSP (Content Security Policy) lagi ho, toh hum direct window object ya alert(1) nahi chala sakte, kyunki CSP in cheezon ko block kar deta hai. Lekin agar woh input field kisi event jese focus ko handle karta ho, toh hum us event ka use karke $event.path access kar sakte hain — jo ek array hota hai jisme input se le kar last mein window object tak saari chain hoti hai. Fir hum AngularJS ka orderBy: filter use karte hain jisse ek sorting operation simulate hoti hai. Is filter ke andar hum .constructor.from([1], alert) ka use karte hain — jo secretly ek alert(1) chala deta hai bina directly window.alert() likhe. Is tareeqe se hum AngularJS sandbox aur CSP dono ko confuse karke XSS achieve kar lete hain.
