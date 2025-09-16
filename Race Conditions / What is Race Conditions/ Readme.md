### Race Condition Kya Hai?
- Jab ek website ek hi data ko ek saath do alag alag threads (processes) se handle karti hai, toh yeh dono threads ek doosre se takra sakte hain. Isse application mein galat cheezain ho sakti hain.
- Masal ke liye, soch ke tum ek online store se kuch khareed rahe ho aur ek discount code use kar rahe ho jo sirf ek baar kaam karta hai. Agar tum is code ko ek hi waqt mein do baar use karo, toh website confuse ho sakti hai aur dono requests ko accept kar leti hai. Yeh ek race condition hai.

### Race Window Kya Hai?
- Yeh woh chhota sa time hota hai jab yeh "takrao" ho sakta hai. Masalan, jab website check karti hai ke tumne code use kiya ya nahi, aur uske database ko update karne ke beech ka waqt. Yeh waqt bohot chhota, jaise ek second ka hissa, ho sakta hai.

### Isse Kya Problem Hoti Hai?
- Agar koi attacker is race window ka faida uthaye, toh woh application ke rules ko tod sakta hai. Masalan:
  - Ek discount code ko baar baar use karna.
  - Gift card ko multiple times redeem karna.
  - Account se zyada paisay transfer karna.
  - CAPTCHA ko ek hi baar solve karke baar baar use karna.

### Limit Overrun Race Condition
- Yeh ek common type hai jisme tum application ke kisi limit ko cross kar dete ho. Masalan, ek online store ka discount code jo sirf ek baar use hona chahiye, lekin agar tum do requests ek hi waqt mein bhejo, toh website dono ko accept kar leti hai kyun ke usne pehli request ko database mein update nahi kiya hota jab doosri request aati hai.

### Isko Kaise Pakda Aur Test Kiya Jata Hai?
- **Step 1**: Pehle ek aisa endpoint (website ka feature) dhoondho jo single-use ya limited ho, jaise discount code ya gift card redeem karna.
- **Step 2**: Is endpoint pe ek saath kaafi requests bhejo, yeh dekhne ke liye ke kya limit cross ho sakta hai.
- **Challenge**: Yeh requests ka timing perfect hona chahiye taake race window mein takraayein. Yeh waqt bohot chhota hota hai, kabhi kabhi milliseconds ka.

### Burp Suite Ka Role
- Burp Suite ek tool hai jo testers use karte hain race conditions ko pakadne ke liye. Isme **Burp Repeater** ka use hota hai jisse tum ek hi request ko baar baar, tezi se bhej sakte ho.
- Tum is tool se yeh check kar sakte ho ke kya tum kisi limit ko bypass kar paa rahe ho ya nahi.

### Real-World Example
- Ek online store mein ek code hai jo 500 rupees ka discount deta hai, lekin sirf ek baar. Tum checkout pe jate ho, code daalte ho, aur ek hi waqt mein do alag alag tabs se yeh request bhejte ho. Agar website sahi se check na kare, toh dono requests pass ho sakti hain, aur tumhe 1000 rupees ka discount mil jata hai!

### Yeh Seekhna Kyun Zaroori Hai?
- Race conditions dangerous hote hain kyun ke yeh attackers ko rules todne dete hain. Agar tum developer ya tester ho, toh yeh samajhna zaroori hai ke apki website is tarah ke attacks se safe rahe.

---

### Burp Suite 2023.9 Mein Naye Features Repeater Ke Liye
Burp Repeater ab bohot powerful ho gaya hai race conditions test karne ke liye. Pehle, requests bhejne mein network jitter (jaise internet ki delay ya timing issues) problem ban jata tha, lekin ab yeh easily handle hota hai parallel requests bhej ke.

- **Kaise Kaam Karta Hai?** Burp automatically server ke HTTP version ke hisaab se technique adjust karta hai:
  - **HTTP/1 Ke Liye**: Classic "last-byte synchronization" use karta hai. Matlab, multiple requests ek saath bhejta hai, lekin har request ka aakhri byte hold karta hai, phir sab ko ek hi time pe release kar deta hai. Yeh jitter ko kam karta hai.
  - **HTTP/2 Ke Liye**: "Single-packet attack" technique, jo PortSwigger Research ne Black Hat USA 2023 mein pehli baar dikhaya. Ek hi TCP packet mein 20-30 requests complete kar deta hai! Yeh network jitter ko almost zero kar deta hai, kyun ke sab kuch ek packet mein hota hai.

Real-world example: Soch, tu ek banking app test kar raha hai jahan balance check aur transfer ek saath hota hai. Pehle jitter ki wajah se requests alag alag time pe pahunchti thi, ab single-packet se tu 20 transfers ek hi packet mein bhej sakta hai, aur dekh sakta hai ke kya limit overrun hota hai (jaise zyada paisay nikal jaayein).

Yeh feature discovery phase mein bohot helpful hai – bohot saari requests bhej ke dekh, kya kuch galat hota hai. Zyada details ke liye, Burp ke "Sending requests in parallel" section check kar. Aur whitepaper padh: "Smashing the state machine: The true potential of web race conditions" – yeh free hai PortSwigger pe.

### Turbo Intruder Se Limit Overrun Test Karna
Turbo Intruder ek extension hai Burp ka, jo Python-based hai aur complex attacks ke liye best. Ab yeh bhi single-packet attack support karta hai (BApp Store se latest version download kar).

- **Kab Use Karo?** Jab simple Repeater se zyada control chahiye, jaise multiple retries, staggered timing, ya hazaron requests.
- **Steps (HTTP/2 Zaruri Hai, HTTP/1 Nahi Chalega)**:
  1. Target HTTP/2 support karta ho, check kar.
  2. Code mein engine set kar: `engine=Engine.BURP2` aur `concurrentConnections=1`.
  3. Requests ko groups mein daal: `engine.queue(target.req, gate='1')` – jaise 20 requests gate '1' mein.
  4. Sab ko parallel bhej: `engine.openGate('1')`.

Example code (Python snippet):
```
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=1,
                           engine=Engine.BURP2)
    
    # 20 requests gate '1' mein daalo
    for i in range(20):
        engine.queue(target.req, gate='1')
    
    # Sab parallel bhej do
    engine.openGate('1')
```
Yeh template Turbo Intruder ke examples mein mil jaayega: race-single-packet-attack.py. Practice kar, dost – shuru mein Python thoda mushkil lagega, lekin ek baar seekh liya toh pro ban jaayega!

### Hidden Multi-Step Sequences (Chhupe Multi-Step Processes)
Ab advanced part: Kabhi kabhi ek single request peeche se multiple steps chalati hai, jaise login mein MFA (multi-factor auth) check. Yeh hidden "sub-states" banati hai – temporary states jahan app ek cheez allow kar deti hai phir badal deti hai.

- **Kya Problem?** In sub-states ko abuse kar ke tu logic flaws exploit kar sakta hai, limit overrun se aage. Example: MFA bypass – login karo credentials se, sub-state mein session valid ho jata hai lekin MFA enforce nahi, ab sensitive page pe direct jaao. (Agar naya hai, PortSwigger ke 2FA bypass lab try kar.)

Pseudo-code example (vulnerable login):
```
session['userid'] = user.userid
if user.mfa_enabled:
    session['enforce_mfa'] = True  # Abhi yeh nahi set hua
    # MFA code bhej aur form pe redirect
```
Yahan sub-state mein tu logged-in ho lekin MFA bypass kar sakta hai – ek login request aur ek sensitive endpoint request ek saath bhej.

Yeh application-specific hote hain, isliye labs mein practice kar. Wild mein (real apps pe) dhoondhne ke liye methodology follow kar.

### Methodology: Hidden Sequences Ko Detect Karne Ka Tarika
PortSwigger ke whitepaper se summarized – step by step apply kar, efficient rahega. Har step mein Burp use kar.

1. **Potential Collisions Predict Karo**:
   - Poora site map kar (Burp Target se).
   - Questions pooch: Yeh endpoint security critical hai? (Jaise password reset ya payment.) Collision possible hai? (Same data pe do requests, jaise ek hi user ke liye do password resets.)

   - Example: Password reset mein agar do users ke requests same record edit karein, toh collision. Alag records pe nahi.

2. **Clues Ke Liye Probe Karo**:
   - Pehle normal benchmark: Repeater mein requests group kar, "Send group in sequence (separate connections)" se bhej. Normal behavior note kar.

   - Phir parallel bhej: "Send group in parallel" se single-packet (ya last-byte) use kar jitter zero karne ke liye. Ya Turbo Intruder.

   - Clues dhoondh: Response change? Email different? App behavior badla? Kuch bhi deviation clue hai. (Pro tip: Burp Professional mein "Trigger race conditions" custom action use kar – one-click parallel requests!)

3. **Concept Prove Karo**:
   - Samajh kya ho raha: Extra requests hatao, phir bhi effect replicate ho?
   - Advanced races unusual effects dete hain, jaise structural weakness. Maximum impact dhoondh – whitepaper padh for details.

### Multi-Endpoint Race Conditions
Yeh simple lagte hain: Multiple endpoints pe ek saath requests bhej ke race window align karo.

- Example: Online store mein item cart mein add karo, pay karo, phir extra items add kar ke force-browse se confirmation page pe jaao. Payment validate hone aur confirmation ke beech window mein extra items slip ho sakte hain.

- Challenge: Windows align karna mushkil, even single-packet se. Solution: Connection "warm" karo – pehle harmless requests bhej (jaise homepage GET), phir Repeater mein "Send group in sequence (single connection)" use kar timing smooth karne ke liye.
