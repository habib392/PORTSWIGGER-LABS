# Yeh kis tarah ki technique thi?
DOM based AngularJs XSS

---

## Important Point:
Agar tu website ke inspect (Elements) section mein ng-app dekh le, toh samajh ja ke yeh AngularJS use kar rahi hai. ✅

---

### ✅ Important Questions & Answers from Lab: 
DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

**Q1: Is lab mein AngularJS kaunsa version use ho raha hai?**

A: Version 1.7.7

**Q2: AngularJS ko kaise detect karte hain page pe?**

A: ng-app attribute ya AngularJS .js file se

**Q3: Source kya tha is lab mein?**

A: Search input field (user ka input)

**Q4: Sink kya tha is lab mein?**

A: AngularJS expression (hidden in runtime, not visible in static HTML)

**Q5: Kya " (double quote) encode ho raha tha?**

A: Haan, " " encode ho kar &quot; ban raha tha (isliye bypass ki zarurat thi)

**Q6: Inspect ya page source mein {{}} kyu nahi dikh raha?**

A: Kyunki AngularJS expressions runtime par evaluate hote hain, statically visible nahi

**Q7: Kis tarah ka XSS hai yeh lab mein?**

A: DOM-based XSS using AngularJS expression injection

**Q8: Payload kya tha jo XSS trigger karta hai?**

A: ```{{$on.constructor('alert(1)')()}}```

**Q9: Is payload mein double quotes (") ki zarurat kyu nahi thi?**

A: Kyunki AngularJS expressions bina quotes ke bhi JavaScript execute kar sakte hain

**Q10: Real-world AngularJS apps mein aisi XSS ka chance kab hota hai?**

A: Jab developer user input ko ```{{ }}``` ke andar directly inject kare bina sanitization ke

**Q11: DOM-based XSS detect karne ka real-life method kya hai?**

A: User input ka JavaScript ya HTML mein reflection dekho (via innerHTML, eval, etc.)

**Q12: AngularJS expressions mein function execute kaise karte hain?**

A: Constructor method se: constructor('code')() format use hota hai

**Q13: AngularJS expression payloads ke kuch aur example kya ho sakte hain?**

A:
```{{constructor.constructor('alert(1)')()}}```

```{{[].__proto__.constructor('alert(1)')()}}```
