
# element.innerHTML kya hota hai?

📌 Easy definition:

Ye JavaScript ka ek tareeqa hai jisse hum kisi HTML box (element) ke andar naya content daalte hain.

### 🧠 Apni Zuban Mein:

Socho tumhare paas ek box hai webpage pe,jaisy searchbox iuska naam hai **result**

Tum JavaScript se uske andar kuch likhna chahte ho, jaise koi word, image ya HTML
Tum likhte ho:

```document.getElementById("result").innerHTML = "Khan";```

➡️ Ab us box ke andar "Khan" likha hua aayega ✅

---

### 🔥 Lekin agar tumhara input attacker se aaya ho?

```document.getElementById("result").innerHTML = userInput;```

```Aur userInput = "<script>alert(1)</script>"```

➡️ Toh script chalayega page pe = XSS 💣
