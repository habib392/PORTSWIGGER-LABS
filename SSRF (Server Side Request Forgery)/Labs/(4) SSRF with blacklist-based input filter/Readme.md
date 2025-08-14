## SSRF with blacklist-based input filter

1. **Product Page Open Kar**

   * Lab open kar, koi bhi product select kar, "Check stock" button pe click kar.
   * Burp Suite me request intercept kar le (Proxy tab me).

2. **Request Repeater Me Send Kar**

   * Intercepted request ko Burp Repeater me bhej de taake hum easily URL change karke test kar saken.

3. **Normal localhost Try Kar**

   * `stockApi` parameter me jo URL hai usko `http://127.0.0.1/` kar ke send kar.
   * Yeh block ho jayega kyunke blacklist me 127.0.0.1 ya localhost block hoga.

4. **127.1 Trick**

   * Ab isko `http://127.1/` kar ke send kar.
   * Yeh kaam karega kyunke `127.1` bhi `127.0.0.1` ka hi short form hai, lekin filter me shaamil nahi.

5. **Admin Panel Access**

   * Ab `http://127.1/admin` try kar.
   * Yeh block ho jayega kyunke filter "admin" word detect kar raha hoga.

6. **Double URL Encoding Trick**

   * "admin" me jo `a` hai usko double URL encode kar:
     `a` → `%61` → `%2561`
   * Final URL:

     ```
     http://127.1/%2561dmin
     ```

7. **Carlos Delete Kar**

   * Burpsuite ky repeater main Stock parameter main yeh url daal "http://127.1/%2561dmin" or send kr request 

8. **Lab Solved**

   * Lab success message aayega.

---

💡 **Penetration Testing Point**:
Real-world me agar server side request me blacklist filtering ho, to tum encoding, alternate IP notations (127.1, decimal, octal), DNS rebinding, ya URL parsing tricks use karke bypass kar sakte ho.
