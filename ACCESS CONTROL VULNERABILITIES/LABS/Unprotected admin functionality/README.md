# Lab: Unprotected admin functionality
Lab open karo (jaise: https://ac8c1f1e1f7c1234.web-security-academy.net)

Us URL ke end pe /robots.txt add karo:

**https://ac8c1f1e1f7c1234.web-security-academy.net/robots.txt**

Is file mein dekho Disallow: /administrator-panel likha hoga

Iska matlab: website ne search engines ko bola hai ke is URL ko crawl mat karna — lekin publicly accessible hai!

Ab /robots.txt ki jagah URL mein yeh likho:

**/administrator-panel**

### Example:

**https://ac8c1f1e1f7c1234.web-security-academy.net/administrator-panel**
Admin panel open ho jaayega 😎

Wahan "carlos" user ko delete karne ka option hoga — click karo ✅

Lab solved!

Real Life Penetration Testing Tip:
Jab bhi koi website test karo to robots.txt zaroor check karo — kabhi kabhi wahan se hidden URLs mil jaate hain jo admin ya sensitive areas dikhate hain.
Ye ek information disclosure vulnerability bhi hoti hai.

