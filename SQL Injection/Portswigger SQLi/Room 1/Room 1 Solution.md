## Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

This lab contains a SQL injection vulnerability in the product category filter. When the user selects a category, the application carries out a SQL query like the following:

SELECT * FROM products WHERE category = 'Gifts' AND released = 1
To solve the lab, perform a SQL injection attack that causes the application to display one or more unreleased products.

### Solution 
 Just go the gift category filter and modify the URL parameter too add ' OR 1=1 -- press enter and LAB Solved

 ### Portswigger Solution 
 Use Burp Suite to intercept and modify the request that sets the product category filter.
Modify the category parameter, giving it the value '+OR+1=1--
Submit the request, and verify that the response now contains one or more unreleased products.
