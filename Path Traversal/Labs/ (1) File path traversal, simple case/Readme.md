### Lab solve — step by step (Urdu, short)

1. **Lab ka maqsad samjho**
   Path-traversal hai: product image request mein `filename` parameter ko misuse kar ke server ki filesystem se files nikalni hain. (Sirf lab ya authorised scope.)

2. **Recon / browse normally**
   Browser se product page kholo jahan image load hoti ho. Image URL ko note karo — aksar `?filename=...` ya similar hota hai.

3. **Intercept request (Burp use karo)**
   Burp Proxy on karo, browser traffic intercept karo aur woh request capture karo jo image fetch kar rahi hai.

4. **Parameter locate karo**
   Request mein `filename` ya jo bhi path param ho, use identify karo. Yeh woh jagah hai jahan traversal payload daalna hain.

5. **Payload modify karo (lab solution)**
   (User ne khud diya) `filename` ko change kar ke traversal sequence bhejo — example: `../../../etc/passwd`. Send/forward karo aur response dekho. Agar file contents aa rahi hain to lab solve ho jayega.

6. **Response verify karo**
   Response mein `/etc/passwd` jaisi lines (user accounts) nazar aayengi — confirm karo ke full content mil raha hai.

7. **Cleanup / restore**
   Burp intercept off karo, browser cache clear kar lo agar zarurat ho. (Lab hygiene.)

8. **Report & fix advice (important)**

   * Input ko whitelist karo (allowed filenames only).
   * User input ko canonicalize + normalise karo (remove `../`).
   * File access ko application user se alag rakhna; use safe root directory and `chroot`/access controls.
   * Use server-side mapping: IDs → internal safe filenames, na ke raw filenames from user.

### Pentest perspective — kya seekhne ko mila (short, real example)

* Yeh vulnerability **information disclosure** deti — attacker sensitive files padh sakta hai.
* Real company example: agar `config.php` ya `credentials` read ho jayein to bahut badi issue.
* Pentester ke liye ye simple but critical check — har file-request endpoint test karo (images, downloads, logs).

### Detection aur mitigation checklist (quick)

* Kya endpoint user input leta hai file path ke liye? → test karo.
* Normalize kar ke `..` blocks detect karo.
* Serve files only from allowed folder using a fixed mapping (ID → filename).
* Proper file permissions on server (sensitive files inaccessible).

---

### 1. **Endpoint**

* **Matlab:** Website ya API ka woh **address (URL)** jahan se koi feature ya data milta hai.
* **Example:**

  * `https://site.com/getImage?filename=pic.jpg` → yeh ek endpoint hai.
  * Agar tum test karna chahte ho ke koi image download ho rahi hai, to tum is **endpoint** par request bhejte ho.
* **Pentest point:** Har endpoint test karo, kyunki har jagah se alag vulnerability mil sakti hai.

---

### 2. **Mitigation**

* **Matlab:** Vulnerability fix karne ya rokne ka **tariqa / solution**.
* **Example:**

  * Problem: User `../../../etc/passwd` dal kar sensitive files access kar raha hai.
  * Mitigation: Sirf **whitelist filenames** allow karo, ya har request ko secure folder me limit kar do.
* **Pentest point:** Jab bhi exploit samjho, uska mitigation bhi likho — report me dikhata hai tum sirf todna nahi, balki secure karna bhi jaante ho.

---

### 3. **Canonicalize**

* **Matlab:** File path ko uski **real, clean form me convert karna** (normalize karna).
* **Example:**

  * User input: `../../secret/../etc/passwd`
  * Canonicalized path: `/etc/passwd`
  * Matlab extra `../` ya `.` remove karke asli path nikalna.
* **Pentest point:** Agar developer canonicalization kare to attacker ka traversal detect ho jata hai, aur usko block kiya ja sakta hai.

---

💡 **Root samajh lo:**

* **Endpoint** = gate (darwaza jahan se request jati hai).
* **Mitigation** = taala (us darwaze ko secure karne ka solution).
* **Canonicalize** = safai (path ko saaf karke asli location samajhna).
