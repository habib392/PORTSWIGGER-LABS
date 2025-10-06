Solving the Single-Endpoint Race Condition Lab
This guide explains how to solve the "Single-endpoint race conditions" lab from Web Security Academy. The goal is to exploit a race condition in the email change feature to set your account email to carlos@ginandjuice.shop, gain admin access, and delete the user carlos.
Prerequisites

Burp Suite 2023.9 or higher
Login credentials: wiener:peter
Access to an email client for @exploit-<YOUR-EXPLOIT-SERVER-ID>.exploit-server.net emails

Steps to Solve the Lab
Step 1: Log In

Go to the lab website and log in with the credentials:
Username: wiener
Password: peter


Step 2: Test Email Change

Navigate to the /my-account page.
Find the email change feature, which shows your current email (e.g., wiener@0a91006f03de366e81b9cf5900aa006f.web-security-academy.net).
Change the email to a test address like test1@exploit-<YOUR-EXPLOIT-SERVER-ID>.exploit-server.net.
Submit the request and check the email client.
Click the confirmation link in the email to verify the email change.
Return to the /my-account page to confirm the email has updated.

Step 3: Capture the Email Change Request

Enable Burp Suite to intercept traffic.
Go to the HTTP history tab in Burp Suite and locate the POST /my-account/change-email request.
Send this request to Repeater for further testing.

Step 4: Create Duplicate Requests

In Repeater, right-click the POST /my-account/change-email request and select Duplicate tab to create a second tab.
In the first tab, keep the email parameter as it is (e.g., test1@exploit-<YOUR-EXPLOIT-SERVER-ID>.exploit-server.net).
In the second tab, change the email parameter to carlos@ginandjuice.shop.

Step 5: Send Parallel Requests

Select both tabs in Repeater and add them to a new group (right-click and select Add to new tab group).
Send the group of requests in parallel (select Send group in parallel).
Check the response in Repeater; you should see a 302 Found status, indicating the requests were processed.

Step 6: Check Email Client

Go to the email client and look for a confirmation email.
If the email body shows carlos@ginandjuice.shop, click the confirmation link to update your account's email.
If the email body shows the test email (e.g., test1@exploit-<YOUR-EXPLOIT-SERVER-ID>.exploit-server.net), repeat Step 5 until you receive a confirmation email with carlos@ginandjuice.shop.

Step 7: Access Admin Panel

Return to the /my-account page. If the email has been updated to carlos@ginandjuice.shop, you should see a link to the admin panel.
Click the link to access the admin panel.

Step 8: Delete User Carlos

In the admin panel, locate the user carlos and delete their account.
The lab will display a "Solved" message upon successful deletion.

Notes

The race condition occurs because parallel requests can cause the server to send a confirmation email to the wrong address due to overwriting the pending email in the database.
If the exploit doesn't work on the first try, repeat the parallel requests (Step 5) as timing is critical.
Ensure Burp Suite is configured correctly to send requests in parallel.

Conclusion
By exploiting the race condition in the email change feature, you can claim the carlos@ginandjuice.shop email, gain admin access, and complete the lab. Practice with Burp Suite's Repeater to master this technique!
