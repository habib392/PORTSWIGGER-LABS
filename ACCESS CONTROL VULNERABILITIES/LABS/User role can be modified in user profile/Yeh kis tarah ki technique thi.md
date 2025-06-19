# 🧠 Technique Used (Simple Words)
Broken Access Control

Application client-side data (JSON body) se role update hone de rahi thi

Server ne ye verify nahi kiya ke koi normal user apni roleid badal raha hai

Ye major logic flaw hai kyunki koi bhi user Repeater se role 2 (admin) bana sakta hai

## 💡 Lesson:
Kabhi bhi critical roles (jaise admin) user input se directly assign nahi hone chahiyein. Har time server ko khud check karna chahiye ke:

User ka real role kya hai?

Kya usko yeh action perform karne ki permission hai?

### Penetration Testing Tip
Jab bhi kisi web app ka profile update ya settings section dekho jahan email, name ya phone update hota hai — us request ko Burp Repeater mein bhej kar check karo ke koi hidden ya sensitive fields (like roleid, isAdmin, permissions) accept toh nahi ho rahi backend se.

✅ Bohat baar developers client-side pe field hide kar dete hain, lekin server-side validation nahi lagate.

🔎 Real-World Target:
"role": "user" → try changing to "role": "admin"

"isAdmin": false → try changing to true

"permissions": ["read"] → try adding "write" or "delete"

Aisi testing se tum Privilege Escalation detect kar sakte ho — jo high severity bug hoti hai real-world bug bounties aur jobs mein.

