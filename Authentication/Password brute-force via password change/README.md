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

