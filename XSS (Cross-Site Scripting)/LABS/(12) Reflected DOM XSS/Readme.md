## 🧪 Lab: Reflected DOM XSS

### 🔥 Summary:

This lab demonstrates a reflected DOM XSS vulnerability via a JSON response that is processed unsafely using JavaScript's `eval()` function. The goal is to inject a payload that breaks out of the JSON string and executes `alert(1)`.

---

### ✅ My Practical Walkthrough:

Main ne Burp Suite ka **Intercept** feature ON nahi kiya tha. Seedha lab website pe gaya aur search bar mein `"XSS"` likh kar search kiya.

Phir maine **Burp Suite > HTTP History** tab mein jaa kar woh request dekhi. Request ko **Send to Repeater** kiya. Wahaan response mein mujhe kuch aisa mila:

```json```
```{"searchTerm":"\"XSS\"", "results":[]}```

🔍 My Analysis:

Mujhe samajh aaya ke search term JSON string ke andar hai.

Agar mujhe XSS run karni hai to mujhe string ke andar se bahar nikalna hoga.

JavaScript mein agar koi string " "  mein ho, aur mujhe uske bahar nikalna hai, to mujhe escape character \ use karna hoga.

So, maine yeh payload banaya:


```\"-alert(1)}//```

🧠 Reasoning Behind the Payload:

\" → Escape karke string ke bahar nikal gaya.

-alert(1) → JavaScript ka function call inject kiya.

} → JavaScript ka block close kiya (just like closing a JSON object or function block).

// → Jo baaki code hota, usko comment out kar diya taake koi error na aaye.


🧪 Final Step:

Maine yeh payload search bar mein daala:

```\"-alert(1)}//```

Jaise hi search kiya, alert(1) trigger ho gaya aur lab solve ho gaya ✅


---

🧾 Key Learnings:

JSON responses agar eval() ke through parse ho rahe hoon, to XSS possible hoti hai.

Backslash \ ka misuse karke JavaScript strings se bahar nikla ja sakta hai.

JavaScript blocks ({}) ko close karna zaroori hota hai, warna syntax error aata hai.

// se baaki ka JSON response comment out kiya jaa sakta hai.

DOM-based reflected XSS mein runtime behavior samajhna sabse important hota hai.



---

🛠️ Final Payload:

```\"-alert(1)}//```
