**Brackets (templates) kaise lagate hain — safe probes (non-destructive)**
Alag engines ka syntax thoda different hota hai, lekin aam tor par double curly braces `{{ }}` common hain (Jinja2, Twig). FreeMarker ya kuch engines mein `${...}` ya `<%= ... %>` type hota hai. Tum safe probe yun bhejo:

* `{{ 7*7 }}`  → agar page par **49** nazar aaye to template evaluate ho raha hai.
* `{{ 'test' + 'ing' }}` → agar **testing** aa jaye to evaluate hua.
* `{{ 3 + 4 }}` → expect **7**.
* For FreeMarker try: `${7*7}` (agar app FreeMarker use karti ho to).

**4) Kaise confirm karo — step-by-step (ethical)**

1. Pehle pata karo input kaha reflect hota hai (comment, search, profile).
2. Harmless probe bhejo (jaise `{{7*7}}`) — **koi destructive command mat bhejo**.
3. Response dekho: agar numeric/string result dikhe ya template-error (engine ka naam / stack trace) milay → strong sign.
4. Agar kuch bhi change na ho aur probe plain text ke tarah show ho → ho sakta hai output escaped ya engine ne treat nahi kia.
5. Hamesha authorization ke saath hi test karo — bina permission production pe mat khelo.

**5) Kya positive result dikhe to kya matlab**

* Agar `49` ya `testing` aa gaya: template engine ne tumhara input **evaluate** kia — potential SSTI.
* Agar error me engine ka naam, function names ya trace aaye: strong indicator.
* Agar probe exactly waisa hi visible raha (escaped): SSTI nahi hua, ya output escaped.

**6) Short safety reminder**
Root tak jao — samjho ke input kaha ja rahi hai aur kyun engine usko evaluate kar raha hai. Aur phir fix karo: escape, sandbox, whitelist. Aur bhai — bina permission production pe test mat karna.

---

**Mitigation ka matlab**
Mitigation = problem ko kam karna ya rokna. Matlab developer ki taraf se wo steps jo vulnerability ko safe banayen — jaise input ko escape karna, template engine ko sandbox karna, unsafe features disable karna, aur user input ko kabhi `eval`/`exec` jaisa treat na karna.

**Evaluate ka matlab**
Evaluate = code ya expression ko execute karna / run karna. Jab template engine tumhara input evaluate karta hai to woh usko calculate karke result page par dikha deta hai (na keval plain text treat kare). Example: agar engine `{{ 7*7 }}` ko evaluate karega to page par `49` aa sakta hai.
