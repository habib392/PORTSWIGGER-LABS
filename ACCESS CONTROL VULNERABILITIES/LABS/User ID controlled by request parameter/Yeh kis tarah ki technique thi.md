# Yeh kis tarah ki technique thi 

### 🔍 Vulnerability Type:
Horizontal Privilege Escalation via insecure access control using GUIDs (Globally Unique Identifiers).

### 🧪 Lab Summary:
Website users are identified using GUIDs instead of simple IDs.

Apni profile access karne ke liye URL hota hai:

**/my-account?id=<user-guid>**

Agar kisi doosray user ka GUID mil jaaye (e.g., Carlos), to uska data bhi access ho sakta hai — access control fail ho jata hai.

### 💡 Real-World Use:
Bohat si real-world websites mein users ka data access hota hai via GUIDs.

Agar yeh GUIDs kisi public jagah (comments, profiles, review URLs) mein mil jaayein, to attacker unko use karke doosray users ke data tak pahunch sakta hai.

Yeh technique kaam karti hai jab server side access control weak ho aur sirf parameter pe rely karta ho.

### 🔐 Pen Testing Tip:
Jab bhi tumhe application mein GUIDs dikhein:

Dekho kya wo kahin aur leak ho rahe hain (comments, links).

Burp Suite ya browser se request intercept karke doosray user ka GUID try karo.

Agar data mil gaya, to Horizontal Privilege Escalation confirm hai.

### 🛡️ Prevention:
Server side par proper access control enforce karo.

Sirf user ID check karna kaafi nahi — server ko verify karna chahiye ke request karne wala banda wahi user hai ya authorized hai.

