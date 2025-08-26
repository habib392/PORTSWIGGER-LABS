# Command Injection Operators

## 1. Semicolon `;`

* Kaam: Pehli command khatam karo aur agli start karo.
* Example:

  ```
  ping -c 1 127.0.0.1; whoami
  ```

👉 Server pehle ping karega, phir user ka naam dikhayega.

PenTesting use: Agar ek command chal rahi hai to uske baad apni extra command inject kar do.

---

## 2. AND Operator `&&`

* Kaam: Doosri command **tabhi chalegi** jab pehli successful ho.
* Example:

  ```
  ping -c 1 127.0.0.1 && whoami
  ```

👉 Server genuine ping karega, phir tumhari command bhi execute hogi.

PenTesting use: Server ko bewakoof banao by adding safe command pehle.

---

## 3. OR Operator `||`

* Kaam: Doosri command **tabhi chalegi** jab pehli fail ho jaaye.
* Example:

  ```
  ping -c 1 999.999.999.999 || whoami
  ```

👉 Ping fail ho gaya (kyunki IP galat hai), to tumhari command execute ho gayi.

PenTesting use: Intentional galti likho aur apni command forcefully chala lo.

---

## 4. Pipe Operator `|`

* Kaam: Ek command ka output doosri command ka input ban jaata hai.
* Example:

  ```
  cat /etc/passwd | grep root
  ```

👉 Pehle pura file ka content aaya, phir sirf "root" wali line nikal gayi.

PenTesting use: Output chhota aur filtered banake detection kam karo.

---

## 5. Command Substitution `$( )` ya Backticks \`\` ` ` \`

* Kaam: Ek command ke andar doosri command chalana.
* Example:

  ```
  echo $(whoami)
  ```

👉 Pehle `whoami` chala, uska result `echo` ne print kar diya.

PenTesting use: Agar `whoami` block hai, to `echo $(whoami)` se filter bypass ho jaata hai.

---

## 6. `echo`

* Kaam: Jo value ya text do, wo print kar deta hai.
* Example:

  ```
  echo Hello Habib
  ```

Output: `Hello Habib`

PenTesting use: Hidden command output ko safe tarike se dikhana.
