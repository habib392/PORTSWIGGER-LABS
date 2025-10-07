Single-Endpoint Race Condition Lab: Analysis aur Key Insights

1. Yeh Kis Tarah Ki Technique Thi?
Yaar, yeh technique race condition exploitation thi, specifically single-endpoint race condition. Is mein hum ek hi endpoint (jaise POST /my-account/change-email) pe multiple requests ek sath (parallel) bhejte hain takay server ke database mein conflict create ho jaye.

Kaise Kaam Karti Hai?: Jab do requests ek hi waqt pe aati hain, server unko handle karte hue confuse ho jata hai. Is lab mein, pehli request email database mein save karti hai, lekin doosri request usay overwrite kar deti hai jab email confirmation task chal raha hota hai. Result? Confirmation link galat email pe chala jata hai, aur hum arbitrary email (jaise carlos@ginandjuice.shop) claim kar lete hain.

Step-by-Step Explanation:

Hum Burp Suite ke Repeater mein requests duplicate karte hain.
Ek request mein test email daalte hain, doosri mein target email.
Parallel send karte hain – yeh timing critical hai, kyunkay server ke threads ek sath run hote hain.
Agar race window hit hoti hai, toh confirmation email galat address pe aa jata hai.


Real-World Example: Banking app mein, agar do users ek hi account se paise transfer karne ki koshish karein ek sath, toh balance double deduct ho sakta hai agar developer ne locking na lagaya ho. Jaise 2010s mein kuch online lotteries mein race conditions se multiple winners ban gaye the!


2. Iss Lab Mein Kya Khaas Baat Thi?
Iss lab ki khaas baat yeh thi ke race condition single endpoint pe thi, matlab sirf ek URL pe multiple concurrent requests se vulnerability exploit hoti hai. Zyada labs multi-endpoints (jaise delete aur create) pe focus karte hain, lekin yeh simple aur realistic tha.

Unique Aspects:

Pending Email Overwrite: Server sirf ek pending email store karta hai per user, aur nayi request purani ko edit kar deti hai instead of appending.
Async Email Task: Email sending background mein hota hai, jo race window create karta hai – database se data fetch hone se pehle overwrite ho jata hai.
Burp Suite Dependency: Lab ko solve karne ke liye parallel requests chahiye, jo Burp 2023.9+ ke group feature se possible hai. Yeh tool-specific tha, jo real pentesting sikhaata hai.


Real-World Example: E-commerce sites pe, cart update mein race condition se item price change ho sakta hai. Jaise Amazon pe early days mein concurrency issues se wrong pricing hoti thi, lekin ab fix hai.


3. Iss Lab Ke Main Points Kya The?
Lab ke main points concurrency vulnerabilities ko highlight karte hain. Yeh step-by-step jaise teacher bata raha hoon:

Predict Collision: Pehle test karo ke multiple email changes se purana confirmation invalid ho jata hai, jo overwrite dikhaata hai.

Benchmark Behavior: Sequence mein requests bhejo (ek ke baad ek) aur dekho sab theek kaam karta hai – no issue.

Probe for Clues: Parallel requests bhejo aur dekho emails mismatch ho jate hain (recipient vs body address).

Prove the Concept: Do requests parallel mein – ek test email, ek target – aur retry karo jab tak confirmation target email ke liye na mile.

Exploit: Confirmation link click karo, admin panel access karo, aur user delete karo.

Key Takeaway: Race conditions timing pe depend karti hain, isliye multiple tries chahiye. Main point tha ke small window mein big impact – admin access without real ownership.

Real-World Example: Social media pe, like button pe race condition se double likes count ho sakte hain, jaise early Twitter pe hota tha.


4. Kya Ghalti Nahi Karni Chahiye Thi Developer Ko?
Developer ko concurrency handling ki ghalti nahi karni chahiye thi. Specifically:

No Locking Mechanism: Database updates pe locks lagaye jaate, jaise row-level locking ya transactions (e.g., SQL mein BEGIN TRANSACTION), takay ek waqt mein sirf ek request update kare.

Ignoring Async Tasks: Email sending async tha, lekin data fetch se pehle check nahi tha ke pending email change nahi hua.

Single Pending Entry: Har user ke liye multiple pending emails allow karte ya unique tokens properly manage karte.

Rate Limiting Miss: Endpoint pe rate limiting lagate takay parallel requests block ho jayein.

Kaise Avoid Kare?: Use mutex locks in code (e.g., Python mein threading.Lock), ya database triggers. Test concurrency with tools like JMeter.

Real-World Example: GitHub pe 2020 mein ek race condition bug tha jahan repo names overwrite ho sakte the – developer ne locking miss kiya, result millions affect hue.


5. Iss Lab Mein Kon Kon Sa Point Weak Ya Vulnerable Tha?
Lab mein yeh points weak the:

Database Design: Sirf ek pending email field per user – yeh overwrite-prone tha. Vulnerable kyunkay no version control ya timestamp check.

Request Handling: Server concurrent requests ko sequentially process nahi karta, balke threads mein, jo race window banata hai (e.g., 1-2 ms ka gap).

Email Template Rendering: Data database se fetch hone ke time pe check nahi ke yeh latest hai – isliye mismatch.

Token Validation: Confirmation token unique hota hai, lekin linked email verify nahi hota properly.

No Input Sanitization for Concurrency: Arbitrary emails allow, lekin concurrency test nahi kiya gaya.

Overall Vulnerability: TOCTOU (Time-of-Check to Time-of-Use) issue, jahan check (email save) aur use (email send) ke beech gap hota hai.

Real-World Example: Online voting systems mein race conditions se votes double count hue hain, jaise kisi US election app mein bug mila tha.


6. Kya Iss Tarah Ki Vulnerability Aaj Bhi Milti Hai Aur Kya Yeh Developer Ki Wajah Se Hoti Hai?
Haan yaar, iss tarah ki vulnerabilities aaj bhi milti hain, especially high-traffic apps mein jahan concurrency zyada hoti hai. Modern cloud systems (jaise AWS) mein bhi, agar code sahi na ho toh yeh issues aate hain. Haan, yeh mostly developer ki wajah se hoti hai, kyunkay woh concurrency ko underestimate karte hain.

Kyun Milti Hai Aaj Bhi?: Microservices aur async programming (e.g., Node.js, Go) mein common hai. Stats ke mutabiq, OWASP Top 10 mein race conditions indirect include hain (under Broken Access Control). 2023 mein, kuch crypto exchanges pe race conditions se funds loss hue.

Developer Ki Wajah Se?: Bilkul – poor code design, no unit tests for concurrency, ya scalability ignore karna. Example: Agar developer single-threaded mindset se code likhe, toh multi-user environment mein fail hota hai.

Kaise Fix Kare?: Use tools like ThreadSanitizer, aur code reviews mein concurrency focus karo.

Real-World Example: 2022 mein Roblox pe ek race condition bug tha jahan items duplicate ho gaye – developer ne async updates miss kiye, result millions ke losses. Aaj bhi, startups mein yeh common hai kyunkay speed over security.

