# User Rate Limiting ka matlab hota hai:

Agar tum kisi website pe bohat zyada login attempts (yaani bar bar username aur password dalny ki koshish) jaldi jaldi karte ho, to website tumhari IP address ko block kar deti hai temporary taur par.

## Block hatne ke 3 tareeqe hote hain:

Kuch waqt guzar jaane ke baad IP khud ba khud unblock ho jaati hai.
Admin manually unblock karta hai.
Tum CAPTCHA solve karo, to unblock ho jaata hai.

User rate limiting ko log account locking se zyada pasand karte hain, kyunki:

Isme attacker easily ye nahi pata kar sakta ke konsa username valid hai (username enumeration se bachao hota hai)

Denial of service ka risk kam hota hai

Lekin yeh system perfect nahi hai.
Attacker apni IP ko badal sakta hai (jaise proxy ya VPN use karke), aur block se bach sakta hai.

Aur agar attacker ek hi HTTP request mein bohot saare passwords try karne ka tareeqa nikaal le, to rate limit ka system fail ho jaata hai.

## Example:
Socho attacker 10 requests kare 10 IPs se — har IP pe 1 try kare — to rate limit ka asar nahi hoga.

