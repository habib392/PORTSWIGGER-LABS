# 🧪 Technique Used

Is lab mein jo vulnerability use hui usay kehte hain:

> 🔥 Broken Access Control via Client-Side Cookie

Website user ke role (Admin ya normal user) ka faisla ek **cookie** ki value se kar rahi thi — jaise:

Set-Cookie: Admin=false

Attacker ne Burp Suite se is value ko **false se true** kar diya:

Set-Cookie: Admin=true

Sirf is chhoti si change ki wajah se, attacker ne **admin panel access** kar liya aur `carlos` user ko delete bhi kar diya.

#### ⚠️ Problem kya thi?
- Server ne check hi nahi kiya ke jo banda request bhej raha hai wo real admin hai ya nahi.
- Sirf cookie par bharosa kar liya, jo ke **client-side** cheez hai aur koi bhi modify kar sakta hai.

#### 💡 Lesson:
Kabhi bhi user ke role ya permission ko **sirf cookies, parameters, ya hidden fields** pe depend nahi karna chahiye. Hamesha server-side par proper check hona chahiye ke:
- Kya yeh user authorized hai?
- Kya isko yeh action perform karne ki ijazat hai?

#### 🔐 Real-World Term:
- Broken Access Control
- Client-side enforced authorization
- Forgeable cookie / parameter tampering
