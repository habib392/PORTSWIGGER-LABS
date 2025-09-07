# Question-Answers

## Question 1
Yeh Template, Embedded Ruby, yeh sub kya hain inka matlab kya hai yeh website main kaisy show hoty hain??

---

## Ans

### 🔹 Template kya hai?

Template asal mein **website ka ek design aur structure** hota hai jisme data dynamically fill hota hai.
Example:

* Ek e-commerce site pe product page ek **template** hota hai.
* Template mein placeholders hote hain jahan actual product ka naam, price, aur description inject hota hai.

So template ka kaam hota hai → **design fix hai, bas data badalta hai.**

---

### 🔹 Embedded Ruby (ERB) kya hai?

ERB ek **template engine** hai jo Ruby programming language use karti hai.

* Matlab tum normal HTML likho, aur uske andar Ruby code inject kar do.
* Jab server isko process karega, woh Ruby code execute karega aur result HTML page mein dikhayega.

Example ERB code:

```erb
<h1>Welcome <%= username %></h1>
```

Agar `username = "Habib"` hai, toh website pe yeh render hoga:

```
Welcome Habib
```

---

### 🔹 Website mein kaise show hota hai?

1. User browser se request bhejta hai.
2. Server template engine (ERB) ko bolta hai: “is HTML template ko Ruby code ke sath process kar.”
3. ERB code ke andar jo `<%= %>` likha hai, woh evaluate hota hai.
4. Final output ek normal HTML ban jata hai jo user ko dikhata hai.

Example:
Template:

```erb
<p>2 + 3 = <%= 2+3 %></p>
```

Browser pe render hoga:

```
2 + 3 = 5
```

---

### 🔹 Problem kaha hoti hai?

Agar developer ne user ka input bina check kiye template mein inject kar diya, toh attacker apna code inject karke server pe kuch bhi chala sakta hai → isko **Server-Side Template Injection (SSTI)** kehte hain.

---

## Question 2
matlab yeh aik website ka page hai 
https://0ab000a204bd47b3b03d03c1005000b2.web-security-academy.net/product?productId=2

iss page main jo product ki image dekh rhi hai, name dek rha ha, or description bhi yeh sub mila kr iss pooray page ko aik template bna dety hain ab btao 

---

## Ans

Haan, woh jo page hai (`/product?productId=2`), uska **poora layout ek template hai**. Dekh:

---

### 🔹 Template ke parts is page par:

1. **Header / Navbar** – jo upar fixed hota hai, sab pages pe same hota hai.
2. **Product Image** – yeh change hoti hai har product ke liye.
3. **Product Name** – alag hoga product 1, product 2, product 3 ke liye.
4. **Product Description** – har product ka unique text hota hai.
5. **Price / Button** – fix design hai, lekin data dynamic hota hai.

---

### 🔹 Template ka kaam

* Developer ek **HTML + ERB template** banata hai, jisme placeholders hotay hain jahan data inject hota hai.
* Database se product ka data fetch hota hai (image, name, description).
* Template engine (ERB) us data ko template ke andar fill karta hai.
* Output → ek complete HTML page jo tumhare browser mein render hota hai.

---

### 🔹 Example (Simplified ERB template)

```erb
<html>
  <body>
    <h1><%= product.name %></h1>
    <img src="<%= product.image %>">
    <p><%= product.description %></p>
  </body>
</html>
```

Agar database mein `productId=2` hai aur uska data hai:

* Name: **Red Shoes**
* Image: **/images/red-shoes.png**
* Description: **Comfortable running shoes**

Toh template render karke banayega:

```html
<html>
  <body>
    <h1>Red Shoes</h1>
    <img src="/images/red-shoes.png">
    <p>Comfortable running shoes</p>
  </body>
</html>
```

---

⚡ Matlab: Page ka **design (structure)** fix hai (template), lekin andar ka **data (image, name, description)** har product ke liye alag hota hai. Isi ko dynamic template rendering kehte hain.

---

## Question 3
yeh placeholders kya hoty hain 

---

## Ans

### 🔹 Placeholder kya hota hai?

Placeholder basically **ek khali jagah (variable)** hoti hai template ke andar jahan **dynamic data inject** hota hai.

Tum socho ek **form** mein jab “Name” likha hota hai aur andar faint text hota hai *Enter your name* → woh bhi placeholder hai. Lekin templates ke case mein iska matlab thoda technical hai.

---

### 🔹 Template Placeholder (Web dev mein)

Template mein developer directly text nahi likhta, balki variable lagata hai jo baad mein database ka data se fill hoga.

Example ERB mein:

```erb
<h1><%= product.name %></h1>
<p><%= product.description %></p>
```

* `<%= product.name %>` = yeh placeholder hai.
* Is waqt khali hai, lekin jab server process karega toh “Red Shoes” inject ho jayega.

Result (Browser pe):

```html
<h1>Red Shoes</h1>
<p>Comfortable running shoes</p>
```

---

### 🔹 Real-world Example

Socho ek “certificate template” hai:

```
This certificate is awarded to: {{student_name}}
For completing the course: {{course_name}}
```

* `{{student_name}}` aur `{{course_name}}` → placeholders hain.
* Jab data inject hoga:

```
This certificate is awarded to: Habib Brother
For completing the course: Cyber Security Basics
```

---

⚡ Matlab: **Placeholder = ek jagah jahan asli data baad mein inject hoga.**

---

## Question 4
Variable kya hai??

---

## Ans

### 🔹 Variable kya hai?

Simple lafzon mein:
**Variable ek dabba (box) hai jisme tum data rakhte ho aur usko naam dete ho.**
Baad mein us naam se data ko call kar sakte ho, change kar sakte ho, ya use kar sakte ho.

---

### 🔹 Real-life Example

Socho tumhari school ki **water bottle** hai:

* Bottle = dabba (variable)
* Naam likha hua hai “Habib” (variable ka naam)
* Andar pani hai (value / data)

Kal pani khatam ho jaye toh tum bottle mein juice daal sakte ho. Matlab **value change ho gayi, variable same hai**.

---

### 🔹 Programming Example (Ruby / Python style)

```ruby
name = "Habib"
age = 19
```

* `name` = variable
* `"Habib"` = value
* `age` = variable
* `19` = value

Agar baad mein:

```ruby
age = 20
```

Toh variable `age` wahi hai, bas value change ho gayi.

---

### 🔹 Web Template Example

Template mein jab `<%= product.name %>` likhte ho → `product.name` bhi ek variable hai jo product ka actual naam store kar raha hai.

---

⚡ Matlab: **Variable = ek naam wali jagah jahan data store hota hai.**

---
