# Header Injection based Access Control Bypass technique thi — specifically "X-Original-URL Header Spoofing".

### 🧠 Yeh technique kis type mein aati hai?
✅ Category:

Access Control Bypass → Server-side Request Smuggling / Header Injection

✅ Technique:

Trusted Header Manipulation — jahan backend application reverse proxy ya load balancer ke lagaye headers ko blindly trust karta hai.

### 🔍 Real-world Example:
Kai frameworks (jaise Apache, Express.js, Nginx reverse proxy) kuch special headers ko route mapping ke liye use karte hain — agar attacker woh headers forge kar le, to woh protected path (like /admin) ko bypass kar sakta hai.

## 💡 Penetration Testing Knowledge ke liye:
Yeh technique tab kaam karti hai jab:

Frontend (like Nginx) path block kar raha ho

Lekin backend (like Node.js, Apache) trusted headers pe request process kar raha ho

Jaise: X-Original-URL, X-Forwarded-Path, X-Rewrite-URL, etc.

### 🛡️ Summary:
“Yeh ek server-side logic flaw hai jahan backend application client-sent headers ko blindly trust karta hai.”

Aisi vulnerabilities critical hoti hain agar:

Admin endpoints expose ho jaayein

Authorization logic sirf frontend tak limited ho

