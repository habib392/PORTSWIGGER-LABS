**Objective**:  
Ek PHP web shell upload karna hai, us se /home/carlos/secret file ka content nikalna hai, aur phir us secret ko lab ke banner mein submit karna hai.

---

### **Step-by-Step Guide**

#### **Step 1: Burp Suite Set Up Karo**
- **Burp Suite chalao**: Pehle apna Burp Suite start karo aur browser ko configure karo taake traffic Burp ke through proxy ho.
- Browser mein proxy settings daal do (usually 127.0.0.1:8080) aur Burp ka Proxy tab on rakho.
- Ye ensure karega ke tum website ke saare requests aur responses dekh sako.

#### **Step 2: Login Karo**
- Lab ki website pe jao aur di gayi credentials (wiener:peter) use kar ke login karo.
- Login ke baad, dekho ke koi option hai jahan se tum avatar image upload kar sako. Ye option usually account settings ya profile section mein hota hai.

#### **Step 3: Ek Random Image Upload Karo**
- Ek random image file (jaise .jpg ya .png) select karo aur avatar upload function se upload kar do.
- Upload hone ke baad, apne account page pe wapas jao. Tumhe apka uploaded avatar page pe dikhega (preview ke taur pe).

#### **Step 4: Burp Proxy Mein Image Request Dekho**
- Burp Suite ke **Proxy > HTTP History** tab pe jao.
- Yahan bohot saare requests honge. Sirf image-related requests dekhne ke liye filter laga do:
  - Filter bar pe click karo.
  - **Filter by MIME type** section mein **Images** checkbox enable karo.
  - Filter apply karo.
- Ab HTTP history mein wo GET request dhoondo jo tumhari uploaded image ke liye hai. Ye request kuch aisa hoga:  
  `GET /files/avatars/<tumhari-image-ka-naam>.jpg`  
- Is request ko right-click kar ke **Send to Repeater** karo.

#### **Step 5: Malicious PHP File Banayo**
- Apne system pe ek simple text file banayo aur isay `exploit.php` naam do.
- Is file mein ye PHP code daal do:
  ```php
  <?php echo file_get_contents('/home/carlos/secret'); ?>
  ```
- Ye code server pe /home/carlos/secret file ka content read karega aur response mein display karega.
- File save karo.

#### **Step 6: PHP File Upload Karo**
- Wapas website ke avatar upload function pe jao.
- Is baar, image ke bajaye apni `exploit.php` file upload karo.
- Upload hone ke baad, website ka response dekho. Agar message aata hai ke file successfully upload ho gayi, to tum sahi track pe ho.

#### **Step 7: Uploaded PHP File Ko Request Karo**
- Burp Suite ke **Repeater** tab pe jao, jahan tumne image ka GET request bheja tha.
- Ab request ka path change karo taake wo tumhari PHP file ko point kare:  
  `GET /files/avatars/exploit.php HTTP/1.1`
- Request send karo.
- Response mein tumhe /home/carlos/secret file ka content mil jayega (ye ek secret code hoga).

#### **Step 8: Secret Submit Karo**
- Jo secret code response mein mila, usay copy karo.
- Lab ke banner mein diya gaya **Submit** button dabao aur secret code paste kar ke submit kar do.
- Agar sab sahi kiya, to lab solve ho jayega, aur tumhe success message milega.

---

### **Important Tips**
- **File Extension Check**: Lab mein koi validation nahi hai, isliye .php file asani se upload ho jayegi. Real-world mein, extensions ko bypass karne ke liye tricks (jaise .php.jpg) use karni pad sakti hain.
- **Burp Repeater Ka Use**: Repeater se request ko bar-bar test kar sakte ho bina website pe manually request bheje.
- **Double-Check Path**: Agar /files/avatars/exploit.php kaam na kare, to confirm karo ke file ka path sahi hai ya nahi (kabhi-kabhi folder structure thoda alag hota hai).
- **Practice**: Ye lab simple hai, lekin real-world scenarios mein validation hoti hai. Isliye Burp Suite aur file upload bypass techniques ke bare mein aur seekho.

