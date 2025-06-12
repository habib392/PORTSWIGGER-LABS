## Lab: Username enumeration via subtly different responses

🎯 Goal:
Enumerate valid usernames using slight differences in server responses.

🧰 Tools: 
- Burp Suite
- Intruder

STEP 1: Open the Lab Open the lab in browser. Make sure Burp Suite is running and intercept is on. Go to /login page of the lab.

🔹 STEP 2: Capture and Send Login Request to Intruder Enter invalid username and password like: username=invaliduser password=invalidpass Submit the form.

In Burp, go to HTTP history, right-click the /login POST request → Send to Intruder.

🔹 STEP 3: Configure Intruder for Username Enumeration Go to Intruder tab → Select the request. You’ll see that Burp already highlights username as payload. Clear the password if needed or keep it static for now (like password=invalid).

🔹 STEP 4: Load the Candidate Usernames List Go to Payloads tab. Select Payload type: Simple list. Paste or load the candidate usernames wordlist.

🔹 STEP 5: Grep the Error Message Go to the Settings tab on the right side.

Expand Grep - Extract → Click Add.

A response viewer will open: Scroll to the line with this message: Invalid username or password. Select the entire message. Burp will extract it from every response. Click OK.

🔹 STEP 6: Start Attack for Username Enumeration Click Start Attack.

Wait for the attack to finish.

After completion: A new column appears showing the extracted error message. Sort that column. You’ll notice one message is subtly different — either missing a period, or has a space like:

Invalid username or password␣ ✅ This indicates a valid username. Note it down.

🔹 STEP 7: Configure Intruder for Password Brute-Force Go back to Intruder tab. Change the position to password only: username=valid-user-found&password=§password§ Go to Payloads tab. Paste or load the candidate passwords wordlist.

🔹 STEP 8: Start Attack for Password Brute-Force Click Start Attack.

Wait for results.

In Status column, find the request that got status 302 (redirect) or a different response length. ✅ This is the correct password. Note it down.

🔹 STEP 9: Login and Solve the Lab Go back to the login page.

Enter: Username: Valid one you found Password: Correct password from brute-force

Submit → You’ll be redirected to the user account page.

🎉 Lab is solved!

# 🧠 What We Learn
Enumeration is the process of identifying valid usernames, emails, or passwords by observing system behavior.
If a login system says "Invalid username" or "Invalid password" separately, it leaks information.
An attacker uses this difference to confirm a valid username.
Then, brute-force can be used to guess the correct password.
Secure systems give generic errors like “Invalid credentials” to prevent this attack.
