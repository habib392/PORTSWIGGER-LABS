Path traversal ek aisi vulnerability hai jisko **directory traversal** bhi kehte hain. Iss main attacker server ke andar ghus ke aise files read kar sakta hai jo asal main uske liye allowed hi nahi hoti.

Jaise:

* Application ka code aur uska data.
* Backend system ke usernames/passwords.
* Operating system ki sensitive files.

Aur agar server weak ho to attacker sirf read hi nahi, balki write bhi kar leta hai (matlab file modify karna). Iss tarah woh application ka data change kar sakta hai, uske behavior ko control kar sakta hai aur aakhri stage pe pura server apne control main le sakta hai.

---

Soch ek shopping website hai jo apne products ki images dikhati hai. Example:

```html
<img src="/loadImage?filename=218.png">
```

Yahan pe `loadImage` wala URL `filename` leta hai aur us file ko dikhata hai jo `/var/www/images/` folder ke andar hoti hai. Matlab agar tu `218.png` bolega to asal file ka path banega:

```
/var/www/images/218.png
```

Ab problem yeh hai ke application ne **path traversal se bachne ka koi defense nahi lagaya**. Matlab attacker URL ko kuch aise badal sakta hai:

```
https://insecure-website.com/loadImage?filename=../../../etc/passwd
```

Jab server isko read karega to path ban jata hai:

```
/var/www/images/../../../etc/passwd
```

Ab jo `../` hai iska matlab hota hai ek level upar jana. Teen dafa `../` likhne ka matlab hua ke `/var/www/images/` se nikal kar root directory tak pohonch gaye, aur wahan se attacker ne `/etc/passwd` file read kar li.

👉 `/etc/passwd` ek Unix file hai jismein system ke users ki details hoti hain. Lekin attacker isi tarike se aur bhi koi file nikal sakta hai.

Windows main bhi yeh same hota hai, bas wahan `../` aur `..\` dono kaam karte hain. Example attack:

```
https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini
```

Yeh Windows ki ek important config file nikal lega.


---

Path traversal exploit karna hamesha asaan nahi hota, kyunke kai applications thodi **defense** laga leti hain. Matlab user ka diya hua `../` sequence block kar deti hain ya strip kar deti hain.

Lekin attacker phir bhi inko **bypass** kar sakta hai alag techniques use karke.

Example: Agar application `../` ko hata bhi de, to attacker seedha hi ek **absolute path** de dega jaise:

```
filename=/etc/passwd
```

Is tarah wo bina traversal sequence ke directly file tak pohonch jata hai. Matlab defense hone ke bawajood bhi attack possible hai.

---

Kabhi kabhi application `../` ko block ya strip kar deti hai. Lekin attacker isko **nested traversal sequences** use karke bypass kar sakta hai.

Example:
Agar tu likhega `....//` ya `....\/`, to application sochti hai yeh normal nahi hai aur `../` ko hata deti hai. Lekin jab system isko process karta hai to yeh phir se simple `../` ban jata hai.

Matlab filter ko bewakoof banake phir bhi traversal ho jata hai.

---

Kabhi kabhi aisa hota hai ke server hi user ka diya hua input clean kar deta hai aur `../` ko strip (hata) deta hai — jaise **URL path** main ya `multipart/form-data` ke andar `filename` parameter main. Matlab application tak traversal pohonchta hi nahi.

Lekin attacker isko **encoding** se bypass kar sakta hai.

👉 Example:

* Normal `../` ko URL encode kardo: `%2e%2e%2f`
* Agar server pehle decode kar de, to double encoding use kardo: `%252e%252e%252f`
* Kabhi kabhi **non-standard encodings** bhi chal jati hain jaise:

  * `..%c0%af`
  * `..%ef%bc%8f`

Aur agar tu **Burp Suite Professional** use karega, to usme **Burp Intruder** ka ek predefined payload list hai — `Fuzzing - path traversal`. Usme ready-made encoded payloads hote hain jo tu try kar sakta hai bypass ke liye.

---

Kabhi application yeh condition laga deti hai ke jo bhi filename user dega woh ek specific base folder se hi start hona chahiye — jaise:

```
/var/www/images
```

Matlab agar tu `filename` bhejta hai to uski value is path se start honi zaroori hai.

Lekin attacker yahan bhi trick kar leta hai. Woh pehle required base folder daal dega aur uske baad traversal sequences use karega.

Example:

```
filename=/var/www/images/../../../etc/passwd
```

Yeh dikhne main to `/var/www/images` se start ho raha hai (jo application ki condition poori kar raha hai), lekin `../../../` use karke phir se root tak pohonch gaya aur sensitive file `/etc/passwd` access kar li.

---

Kabhi application yeh rule laga deti hai ke jo bhi filename tu bhejega uska end ek specific extension se hona chahiye — jaise `.png`. Matlab har file ka naam `.png` pe khatam hona lazmi hai.

Lekin attacker isko bhi bypass kar leta hai **null byte injection** se. Null byte (`%00`) system ko bolta hai “bas yahan path khatam karo”.

Example:

```
filename=../../../etc/passwd%00.png
```

Yahan application sochti hai ke file ka naam `.png` pe end ho raha hai, lekin system `%00` pe file path terminate kar deta hai. Result yeh hota hai ke asal file open hoti hai:

```
/etc/passwd
```

Aur attacker sensitive data nikal leta hai.


---

How To Prevent

**Path traversal attack se bachne ka best tareeqa** yeh hai ke user ka diya hua input kabhi bhi direct filesystem APIs ko pass hi na karo. Matlab user se filename lo hi mat, ya phir application aise design karo ke uski zaroorat hi na pade.

Lekin agar avoid karna mushkil hai, to phir **2 layers ka defense** lagana chahiye:

1. **Input validation:**

   * User ke input ko process karne se pehle check karo.
   * Best option hai ek **whitelist** banani — sirf unhi file names ko allow karo jo tumhe chahiye.
   * Agar whitelist possible nahi, to kam se kam input ko restrict karo (sirf alphabets aur numbers allow karo, special characters block kar do).

2. **Canonicalization + Check:**

   * Jab input le liya to usko base directory ke sath append karo.
   * Us path ko canonicalize (normalize) karo takay system ke shortcuts like `../` resolve ho jayein.
   * Fir check karo ke final path hamesha usi base directory se start ho raha ho. Agar nahi ho raha to request block kar do.

Example (Java code):

```java
File file = new File(BASE_DIRECTORY, userInput);
if (file.getCanonicalPath().startsWith(BASE_DIRECTORY)) {
    // process file
}
```

Yani agar file ka asal (canonical) path tumhare base directory se start karta hai tabhi process hoga, warna attacker ka input reject ho jayega.


