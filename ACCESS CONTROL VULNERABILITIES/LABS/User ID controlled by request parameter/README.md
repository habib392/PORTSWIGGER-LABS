# IMPORTANT MESSAGE MUST LEARN BEFORE SOLVING THE LAB

Kuch websites main jo parameter hota hai (jaise user ID), wo easy guessable number nahi hota — jaise 1, 2, 3… balkay uski jagah wo GUID use kartay hain (Globally Unique Identifier), jo bohot long aur random hota hai, jaisy: 5e9f8a91-3d3e-4a62-a8e2-0b06ec9df2a9.

Iska faida ye hota hai ke attacker asani se kisi aur user ka ID guess nahi kar sakta.

Lekin kabhi kabhi ye GUIDs website ke kisi aur hissay main leak ho jatay hain, jaise user comments, reviews, ya messages main.

Phir attacker unhi GUIDs ko utha kar unauthorized actions kar sakta hai — IDOR (Insecure Direct Object Reference) ka chance ban jata hai.

### 📌 Real world use (Pen Testing tip):
Agar tum application main GUIDs dekh rahay ho, to unko search karo ke kahin aur to leak nahi ho rahay (Dev Tools, API calls, responses, HTML source). Agar mil jayein, to un GUIDs ko sensitive actions main try karo — ho sakta hai access control break ho jaaye!

# 🔐 Lab Name: User ID controlled by request parameter, with unpredictable user IDs

## 🎯 Goal: Carlos ka GUID dhoondhna aur uska API key nikaal kar submit karna

📌 Step 1: Carlos ka GUID dhoondo

Lab open karo.

Left ya main page pe koi blog post ya comment dhoondo jahan Carlos ka naam likha ho.

Carlos ke naam pe click karo (usually clickable hota hai).

Tumhe browser ke URL mein kuch is tarah ka kuch milega:

**/user/5e9f8a91-3d3e-4a62-a8e2-0b06ec9df2a9**

📍 Yeh Carlos ka GUID hai. **Isko copy kar lo.**

📌 Step 2: Apne account se login karo

Credentials:

**Username: wiener**

**Password: peter**

Login karte hi tum apne account page pe jaoge, jahan URL kuch aise hoga:

**/my-account?id=your-guid**

📌 Step 3: Carlos ka ID lagao

Ab tum URL mein id= ke baad apna GUID hatao aur Carlos ka GUID paste karo:

**/my-account?id=5e9f8a91-3d3e-4a62-a8e2-0b06ec9df2a9**

Enter karo — agar vulnerability hai to Carlos ka account page khul jaayega.

📌 Step 4: API Key uthao

Page pe Carlos ka API key ajaye gi

Usay copy karo.

📌 Step 5: Submit the solution

Lab ke interface mein jahan **"Submit solution"** ka field hota hai, wahan wo API key paste karo aur submit karo.

LAB SOLVED

### 🔥 Real-world Pen Testing Tip:
GUID secure lagtay hain, lekin agar wo kahin leak ho jaayein (jaise public profile, messages, comments), to horizontal access control
fail ho jata hai. Tum un IDs ka use kar ke doosray users ke data tak pahunch saktay ho.

Yeh sub GUID url main show hongi


