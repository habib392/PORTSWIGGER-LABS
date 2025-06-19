# 📂 Technique ka naam:
Unprotected Functionality Disclosure via Client-side Source Analysis

### ⚒️ Yeh kis cheez ke under aati hai (category)?

- 🔐 Broken Access Control

- 🕵️ Information Disclosure

- 🔍 Reconnaissance / Enumeration Phase

### 🧠 Technique ka core idea:
Application ke frontend (browser side) ka code (HTML/JS) kabhi kabhi aise URLs, endpoints ya paths bata deta hai jo user ko dikhaye nahi jaate, lekin code mein likhe hote hain — aur attacker unhein easily nikal sakta hai.

### 📊 Real-World Example:

**admin panel is at /super-secret-admin-9237/**
**<script src="/static/js/app.js"></script>**

Ya JS file mein:

**const adminURL = "/super-secret-admin-9237/";**

### ⚔️ As a pentester, iska faida:
Tum hidden pages dhoond sakte ho bina brute force ke

Tum un pages ko access kar ke unauthorized actions perform kar sakte ho

Real bug bounty mein aise disclosures se P1 ya P2 severity bugs milte hain

### 🛡️ Developer ke liye sabak:
Kabhi bhi sensitive paths ko client-side code mein mat likho

Server pe hamesha access control enforce karo — chahe URL hidden ho ya nahi

