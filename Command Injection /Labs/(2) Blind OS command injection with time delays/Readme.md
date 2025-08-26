## Blind OS command injection with time delays

⚡ Step by step Solution

1. **Feedback form submit karo**
   – Koi bhi random data daalo: name, email, comment.
   – Burp Suite ON rakho taake request capture ho.

2. **Request intercept karo (feedback request)**
   – Usme tumhe `email=` parameter milega (ya koi aur user input).

3. **Injection try karo**
   – Email parameter ko modify karo is tarah:

   ```
   test@test.com||ping+-c+10+127.0.0.1||
   ```

   Yahaan `||` ka matlab hai "pehle wali command execute karo, phir meri command bhi chalao".
   `ping -c 10 127.0.0.1` 10 packets bhejta hai → approx. 10 sec delay.

4. **Response check karo**
   – Agar injection successful hai, toh response tumhe 10 sec delay ke baad aayega.
   – Agar instant aa raha hai, toh ya toh payload kaam nahi kar raha ya filter hai.

👉 Lab solve hone ki condition: server ki response main noticeable 10 second delay ho.

---

⚡ Penetration testing point of view:
Real websites main ye technique tab kaam aati hai jab tumhe **command ka direct output nahi milta**. Tum delay ya out-of-band techniques use karke injection prove karte ho.

Chhota hack:

* Linux: `sleep 10` bhi kaam karta hai.
* Windows: `ping -n 10 127.0.0.1` (kyunki wahan `-n` hota hai, `-c` nahi).
