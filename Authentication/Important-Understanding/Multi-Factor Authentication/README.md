# Multi-Factor Authentication (MFA) kya hoti hai?
Yeh wo system hai jisme sirf password (1 cheez) se kaam nahi chalta — balkay tumhain 2 ya zyada cheezen prove karni hoti hain:

- Jo tum jante ho (password)
- Jo tumhare paas hoti hai (mobile, code, device etc.)

Yeh system zyada secure hota hai simple password se, 
lekin agar sahi implement na ho to phir bhi attacker bypass kar sakta hai.

### 2FA ka example:
Tum login karte ho password se

Uske baad tumhare mobile ya email pe ek OTP/code aata hai

Dono sahi do to access milta hai

### Isme kya vulnerabilities hoti hain?
Poor implementation – agar system weak bana ho to 2FA hone ke bawajood attacker andar ghus sakta hai.

Same cheez ka 2 baar check hona – jaise email-based 2FA:

Password bhi email ka

Code bhi email pe
=> Agar attacker ko sirf email ka access mil gaya, to dono factors mil gaye — yeh asli 2FA nahi hoti.

Code intercept ho sakta hai – agar SMS ya email insecure ho to OTP chori ho sakta hai.

Biometric (jaise fingerprint, face scan) websites pe mushkil hai, isliye zyada tar MFA sirf code+password tak hi limited hoti hai.

### Conclusion:
2FA tabhi strong hoti hai jab alag alag type ke factors use kiye jaayein, warna attacker bypass kar sakta hai.
