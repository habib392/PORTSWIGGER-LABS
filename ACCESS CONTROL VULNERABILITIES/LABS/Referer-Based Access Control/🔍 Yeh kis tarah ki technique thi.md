# 🔍 Yeh kis tarah ki technique thi?

## Technique ka naam:
➡️ Referer header manipulation for access control bypass

### Type of vulnerability:
➡️ Broken Access Control
(Specifically: insecure reliance on HTTP headers like Referer)

### 🕵️‍♂️ Penetration tester isko kab use karta hai?
Jab bhi tum kisi web app mein dekho ke:

Admin ya sensitive actions (jaise user promote/delete) hain

Request reject ho rahi hai bina koi reason diye

Ya 403 Forbidden return ho raha hai

Toh Burp Suite mein:

Referer header add karo ya modify karo

Dekho kya request allow ho jaati hai

Yeh test especially tab karna chahiye jab:

Koi GET ya POST request protected lag rahi ho

Auth ka error aata ho bina proper login check ke

Ya jab tum request direct hit karo toh fail ho, lekin kisi link se jao toh pass ho

### ✅ Is lab se tu ne kya seekha?
Referer pe bharosa karna insecure hai: Attacker easily modify kar sakta hai

Burp Suite Repeater powerful tool hai: Tum header modify karke test kar sakte ho

Access control hamesha server-side strong hone chahiye: Header ya client-side controls pe rely nahi karna chahiye

Har request ko individually secure hona chahiye: Sirf main /admin page nahi, balki sub-pages jaise /admin/deleteUser, /admin-roles bhi secure hone chahiyein

Real-world mein attacker ko full access mil sakta hai: Agar sirf Referer pe access diya jaa raha ho

### 🧠 Ek Real-life Socho:
Agar kisi hospital ki admin portal mein /admin/patientDelete?id=123 sirf Referer check kar ke chalta ho, toh attacker patient records delete kar sakta hai — bina admin access ke 😱
