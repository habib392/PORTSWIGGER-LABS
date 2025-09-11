# Server-side template injection with info disclosure

1. Login

Credentials use kar:

content-manager:C0nt3ntM4n4g3r

Abhi tu apny account main login ho ja.

2. Template Edit Karna

Product description ka template open kar ke edit mode main ja.

3. Error Trigger Karna (Engine Identify)

Template ke andar koi invalid payload daal de, jaise:

${{<%[%'"}}%

Save kar ke dekh, error aayega jisme Django framework ka zikr hoga.

Matlab tu confirm ho gaya ke backend Django template engine use kar raha hai.

4. Debug Tag Lagana (Info Disclosure)

Ab error payload hata de aur likh:

{% debug %}

Save kar, ab tu full debugging info dekh lega jisme objects aur unki properties dikh rahi hongi.

5. Settings Object Dhoondna

Debug output ko study kar. Wahan settings object available hoga.

Django documentation check karo, settings ke andar ek property hoti hai:

SECRET_KEY

Yehi attacker ka target hota hai.

6. SECRET_KEY Access Karna

Ab {% debug %} remove kar ke likh:

{{settings.SECRET_KEY}}

Save kar, ab page pe framework ka secret key directly render hoga.

7. Solution Submit Karna

Jo secret key value mili hai, use copy kar aur lab ke solution form main paste kar ke submit kar de.

---

⚡ Pentesting Tip

Ye injection is liye possible hua kyunki developer ne user-supplied object ko template engine ke andar directly expose kar diya bina sanitize kiye.

Agar attacker ko settings ka access mil jaye, woh SECRET_KEY le kar session forgery, CSRF token bypass aur signing bypass kar sakta hai.

Real websites main agar tu aisi debugging directives (debug, dump, print) ko access kar le, toh tu environment variables, API keys aur database credentials tak pohanch sakta hai.


