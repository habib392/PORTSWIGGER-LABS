## Questions/Answers

🔍 Breakdown of Payload:

http://x.com?&apos;-alert(1)-&apos;

🔹 1. http://x.com

Yeh just fake URL hai — tu kuch bhi likh sakta hai, e.g., http://abc

Sirf isliye diya gaya hai taake href aur tracker.track(...) ke andar input valid lage

---

🔹 2. ?

➤ Ye kyun lagaya?

Yeh URL ka query start karne ke liye hota hai

Tu jab URL mein ?something daalta hai, toh iska matlab hota hai: "yeh parameters start ho gaye"

Yahan pe tu query string bana raha hai, lekin asal maqsad hai: JS string break karni hai


##### ❗ Yani iska technical kaam nahi hai yahaan, bas JavaScript string ka structure thoda real banane ke liye use hua hai.

---

🔹 3. &apos;

➤ Yeh kya hai?

&apos; = HTML entity hai jo ' (single quote) ban jaata hai render hone ke baad

Isko likhne ka faida:

Direct ' likhoge toh escape ho jaayega (\') ya block ho jaayega

Lekin &apos; HTML encoding hai, jo browser render karta hai as '

📌 Yani tu filter ko fool kar raha hai:

Filter sochta hai tu ' use nahi kar raha

Lekin browser mein jab page open hota hai, wo &apos; ko ' bana deta hai

---

🔹 4. -alert(1)-

➤ Yeh tera actual XSS payload hai

Jab tu JavaScript string todh deta hai (via '), to uske baad kuch JS likh sakta hai

Tu alert(1) likhta hai, taake confirm ho jaye ke XSS run ho gaya

- ka koi khaas kaam nahi hai yahaan, bas structure normal rakhne ke liye daala gaya hai (optional hai)

---

🔹 5. &apos;

Yeh dobara closing ' banata hai

Tera payload ban gaya:

```tracker.track('http://x.com?'-alert(1)-'');```

Jo JavaScript ke liye ban jaata hai:

```tracker.track('http://x.com?' - alert(1) - '');```

Pehla ' string todta hai

alert(1) chal jaata hai

Phir ' dobara lagta hai taake syntax error na ho

---

🔹 6. ;

Yeh semicolon JavaScript ka statement terminator hai

Aksar use karte hain taake JS ko bataa sake: "yeh statement yahaan khatam ho gaya"