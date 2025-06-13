# 🔐 Lab: Broken Brute-force Protection (IP Block Bypass) — Solved ✅

This lab from **PortSwigger Web Security Academy** demonstrates a logic flaw in brute-force protection. Here's how I solved it step-by-step in easy words.


## 🎯 Objective

1. Carlos ka password brute-force karna
2. Uska account login karna
3. Account page access karna (Lab complete)


## 🧠 Logic Summary

- 🔒 After **3 failed login attempts**, server **blocks IP**
- 🔁 But agar tum **apne (wiener) account se login karo**, to failed attempt **reset ho jati hai**
- 💡 So: har kuch wrong attempt ke baad apna sahi login daalo
- 🛠️ Is logic flaw ka use karke brute-force successful ho jata hai



## 🛠️ Tools Required

- Burp Suite (Community edition)
- Firefox / Chrome (Burp proxy configured)
- Candidate password list (from lab)

## 🪜 Step-by-Step Guide

### ✅ Step 1: Login Page Check

- Burp chalao
- Lab URL open karo
- Login page pe `wiener:peter` se login test karo

### ✅ Step 2: Prepare Payloads

- Payload Set 1 (Usernames):

wiener carlos wiener carlos wiener carlos ...

- Payload Set 2 (Passwords):

peter pass1 peter pass2 peter pass3 ...

- 💡 Har `carlos` ke password se pehle `wiener:peter` login hona chahiye

### ✅ Step 3: Capture Login Request in Burp

- Galat login karo (e.g. test:test)
- Burp HTTP History → `/login` POST request → Right-click → **Send to Intruder**

### ✅ Step 4: Configure Intruder

- **Positions tab** me:
- `username` → `§username§`
- `password` → `§password§`
- **Attack type**: Pitchfork

### ✅ Step 5: Resource Pool Setup

- Resource pool tab open karo
- New pool banao:
- Name: `Slow pool`
- Max concurrent requests: `1`
- Attack ko us pool se assign karo

### ✅ Step 6: Add Payloads

- **Payload Set 1**: Usernames list (alternating `wiener`, `carlos`)
- **Payload Set 2**: Passwords list (alternating `peter`, `passX`)

### ✅ Step 7: Start the Attack

- Attack start karo
- Dheere chalay ga (1 request at a time) — protection bypass ho jayegi

### ✅ Step 8: Analyze Results

- Jab attack complete ho jaye:
- Filter: **Hide status code = 200**
- Sort karo responses → find where **username = carlos** and **status = 302**
- Wahan wala **password** note karo from Payload 2

### ✅ Step 9: Login as Carlos

- Carlos ka username + correct password daalo
- Account page open karo

### 🎉 Step 10: Lab Solved!

- Account access milte hi lab complete ho jata hai 🚀

## 💡 What I Learned

- Brute-force protection logic flaws kis tarah bypass hoti hain
- Burp Suite Intruder ka **Pitchfork attack**
- Resource pool use karna (to slow down attack)
- IP blocking aur counter reset tricks

## 🔗 Tags

#WebSecurity #BurpSuite #EthicalHacking #BruteForce #PortSwigger #CyberSecurityLabs #LearnByDoing`