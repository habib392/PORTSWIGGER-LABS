# Password brute-force via password change
Sabse pehle wiener user add kiya.

Uske baad login kiya, login hone ke baad 3 options aaye:

- Current password
- New password
- Confirm new password

### Maine test kiya:

Agar current password galat daalun (e.g. 2213) aur new password aur confirm password same hoon, to phir main dobara login page pe chala gaya (matlab statement galat thi).

Phir current password sahi daala (e.g. peter) aur new password 2213 aur confirm new password abc (matlab dono different) to mujhe error mila:
"New passwords do not match"

Uske baad BurpSuite open kiya → HTTP history me gaya →
POST /my-account/change-password request dhoondi → usse Send to Intruder kiya.

### Intruder me:

Username ko wiener se change karke carlos kar diya.

Current password ke jagah peter daala.

Payload position bana diya:

**username=carlos&current-password=§peter§&new-password-1=123&new-password-2=abc**
Phir password list se sare passwords copy kiye aur payload list me paste kiye.

## Grep Match me gaya → ye string add ki:
**"New passwords do not match"**

Jab attack start kiya to ek naya column aaya New passwords do not match ka.

Attack finish hone ke baad ek response mila jisme ye column tha.
Iska matlab tha ke yehi correct current password tha.

Fir wo password copy kiya → website pe aaya → Login tab pe gaya:

Username: carlos

Password: (jo brute force se mila)

Login successful → LAB SOLVED ✅

# PART 2
🔍 Lab ka Asal Objective Kya Tha?
Password change functionality ko exploit karna jahan:

Attacker already authenticated hai (jaise wiener).

Lekin usne target user (Carlos) ka password brute force karna hai by misusing the change password endpoint.

✅ Flow Breakdown
Tu wiener user sy login hua — yani tu ek valid user bana.

Phir tu change password page pr gaya — jahan current password + new password daalne ka form tha.

Tu ne form ka behaviour test kiya (negative testing) — ye bohot acha habit hai ek pentester ka.

BurpSuite me change-password request pakra.

Intruder use kiya — aur Carlos ka password brute force kiya using the error message as grepping filter:

"New passwords do not match" ka matlab tha current password sahi hai (jo tu brute force kar raha tha).

Jab tu ne correct current password identify kar liya — tu ne Carlos ka valid login kar liya using brute forced password.

## 🔐 Is Lab ka Real-World Use
Jab password change form validation server-side proper na ho, toh attacker valid error responses ko as an oracle use karke kisi bhi user ka current password brute force kar sakta hai — even without account lockout.

### 🔧 Yeh attack kehlata hai:
**"Username enumeration + password brute force via password change functionality"**

## 🧠 Lesson:
Jab bhi form ho jahan current password daalna hota hai, uska response generic hona chahiye (na ke exact error de).

Always check POST request body — aur test karo kya kisi aur user pe bhi woh form kaam karta hai (parameter tampering).

Burp Intruder + Grep Match powerful combo hai brute force / logic flaw detect karne ke liye.

### 🔥 Tips for Future Labs:
Jab bhi koi functionality dekhay jo login ke baad bhi user-input par depend ho (jaise password reset, email change, etc.) — usme parameter tampering try karna.

Grep match use karo to identify response-based clues.

