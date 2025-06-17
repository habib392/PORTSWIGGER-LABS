# Password reset broken logic
## How To Solve This Lab
Iss lab ki password reset functionality vulnerable hai matlab forgot password
wala option weak hai iss sy ham kisi dosray ka account access kr sakty hain
Hum ny iss lab main apnay account (wiener) ka use kr ky carlos ka account access krna hai 

### Now Solve
My account main jaana hai wahan forgot password option py click krna hai phir apna username wiener add karna hai

isky baad Email Client waly option main jaana hai wahan aik url aaya hoga ius py click karna hai

phir koi bhi new password add kr dena hai

Burpsuite on krna hai phir HTTP history main jaana hai POST /forgot-password ky inder wahan yeh dekhna chahie temp-forgot-password-token

Phir isko Send to Repeater main bhej dena hai 

Repeater main yeh check krna hai ky agr temp-forgot-password-token ka jo code hai iusko agr dono jagah url or body sy remove
kr dein taab bhi wo waisy hi work kare gi jaisy wo iss ky hony sy kr rahi thi matlab request ko send krna hai or response main
dekhna hai ky still 302 code show hoo rha hai ya nhi agr show hoo rha hai too iska matlab token ko sahi tariqay sy check nhi
kiya jaa rha jab new password submit kiya jata hai

Browser main jaana hai password ko forgot kr ky username wiener add karna hai isky baad Email Client
waly option main jaana hai wahan aik url aaya hoga ius py click karna hai
phir koi bhi new password add kr dena hai (Repeated Process)

Isky baad phir POST /forgot-password ky inder jaana hai or ius ko repeater main bhejna hai or wahan iss baar 2 kaam krny hai
aik too temp-forgot-password-token ka jo code hai iusko dono jagah url or body sy remove
kr dena hai or dosra username ko change kr ky carlos kr dena hai or password bhi koi new add kr dena hai

Isky baad request ko send kr dena hai respone main 302 code show hoo jaye ga

Phir website pr jaana hai or account carlos ka login kr lena hai or password bhi wo add krna hai jo abhi add kiya tha

LAB SOLVED


