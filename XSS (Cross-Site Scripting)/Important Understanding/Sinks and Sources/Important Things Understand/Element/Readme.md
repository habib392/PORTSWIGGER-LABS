# Element ka matlab hota hai:

Webpage ka koi bhi part (box, text, image, etc) jisko JavaScript se control kiya ja sakta hai


Jaise:

- ```<div>```
- <p>
- <h1>
- <span>
- <img>
- <input>
- <button>

Yeh sab HTML elements hote hain.

---

###🧪 Real Example:

```<p id="result"></p>```

Yahan <p> ek element hai
Aur iska id="result" hai
Toh JavaScript se tum isko control kar sakte ho:

```document.getElementById("result").innerHTML = "Hello Habib!";```

➡️ Yahan:

element = <p id="result">

innerHTML = us element ke andar ka content