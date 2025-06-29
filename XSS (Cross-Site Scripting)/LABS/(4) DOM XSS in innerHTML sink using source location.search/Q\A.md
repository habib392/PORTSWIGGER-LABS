# Questions/Answers

Mane search main pehly search kiya "khan" or phir view source py click kiya ius ky baad 
yeh javascript aaya 

```<script>```
                          ```function doSearchQuery(query) {
                                document.getElementById('searchMessage').innerHTML = query;```
                           ``` }
                            var query = (new URLSearchParams(window.location.search)).get('search');
                            if(query) {```
                                ```doSearchQuery(query);```
                            ```}
                        </script>```
                        
ab baat yeh hai iss main samj gya main kya scene hai 

bascially iss javascript main variable "query" bana hai jidhar jidhar query likha hua hai wo mera search kiya hua text hai "khan" 
isky baad "document.getElementById" source hai 
or "innerHTML" jo hai na wo sink hai

ab yeh btao ky inn sub ny kis tarah kaam perform kiya jab mane search main "khan" search kiya or meray khayal sy yahan jo search box hai yeh element hai

---

### ANS: Tera sochna 100% correct hai. ✅

🔍 Jab tu ne "khan" search kiya, to peeche kya hua?

✅ 1. URL ban gaya:

```https://example.com/?search=khan```

Isme ```?search=khan = query string```

Tumhara input browser ne location.search ke through JavaScript ko diya.

✅ 2. JavaScript code ne yeh kaam kiya:

```var query = (new URLSearchParams(window.location.search)).get('search');```

Matlab:

```location.search = ?search=khan```

```.get('search') = "khan"```

To ab variable query = "khan" ho gaya. ✅

(aur tu ne bilkul sahi samjha: query ab "khan" hai)

✅ 3. Phir ye function chala:

**doSearchQuery(query);**

Yani:

**doSearchQuery("khan");**

To **doSearchQuery()** function ne "khan" ko andar bheja.

✅ 4. Aur andar ye tha:

```document.getElementById('searchMessage').innerHTML = query;```

Yani:

```document.getElementById('searchMessage').innerHTML = "khan";```

To HTML ka jo element hai jiska id='searchMessage' hai → uske andar "khan" dikh gaya browser pe.

### Summary:

- Search box = input element

- "khan" = user input

- location.search = source

- query = variable jisme input gaya

- innerHTML = sink jahan XSS ho sakta

- getElementById(...) = wo jism jisme input inject ho raha hai





 
