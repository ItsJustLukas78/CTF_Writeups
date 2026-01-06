# Cowsay
 
**Date**: 01-05-26

**Author**: Lukas Somwong  

**Platform**: DAMSEC CTF League

**Category**: SQL Injection

## Overview

There were two flags in this challenge. I was provided two seperate links. Both links directed to a login page, but the second was specifically an "Admin Login" page.


## Solution for Flag 1

Entering any arbitrary combination of a username and password displayed the SQL query that was executed upon each login attempt.

![alt text](<images/Screenshot 2026-01-05 at 8.16.43 PM.png>)

Observing the query, I wondered if I could inject SQL to cause the query to always return the user without needing the correct password.

In the username and password field, I continued to input arbitrary values, but I appended the following to the password field so that the query WHERE clause was always truthy. 

`123' OR 1=1 -- `

![alt text](<images/Screenshot 2026-01-05 at 8.23.30 PM.png>)

I was able to succesfuly log in. The page then prompted me to enter a string for a cow to say. I suspected that the application was using the cowsay command-line program behind the scenes to generate the cow.  

I suspected that I needed to somehow escape the cowsay input to execute commands on the remote machine, where the flag was likely stored on.

I attempted to escape the input string by using a single quote, followed by a semicolon to append terminal commands I wanted to execute, being `ls` in this case to see whats in the currect directory, then ending the input with another semicolon and single quote to execute an empty string which in all creates a single line of valid commands that do not result in error.

`Hello'; ls ; '`

which on the remote machine theoretically executes as

`cowsay 'Hello'; ls ; ''`

![alt text](<images/Screenshot 2026-01-05 at 8.30.18 PM.png>)

I was able to sucessfuly execute `ls` on the remote machine, and the application displayed the output of that command, listing out files and directories in the same directory as the program.

None of the files stood out to me, so I checked the root directory.

![alt text](<images/Screenshot 2026-01-05 at 8.43.34 PM.png>)

In the root directory, I found the flag.txt file and used cat to read the flag

![alt text](<images/Screenshot 2026-01-05 at 8.45.09 PM.png>)

## Solution for Flag 2

Like the previous page, entering any arbitrary combination of a username and password on the admin login displayed the SQL query that was executed upon each login attempt. This time, the password was not included in the query. 

![alt text](<images/Screenshot 2026-01-05 at 8.15.49 PM.png>)

On the bottom of the page, I discovered a link to the source code of the application.

![alt text](<images/Screenshot 2026-01-05 at 8.48.54 PM.png>)

Reading the source code, it appeared that the program first executed a query to check if the entered username appeared in the database, then either immediately returned "The user was not found" or checked the password and returned "That password is incorrect" if it was incorrect and directed to the next page if it was correct.

The page only displayed wheather the user was found, and if the password was correct. I wouldn't be able to write SQL to fetch and display the password. I also wouldn't be able to inject SQL in the password field since the program only compared it to the password that was retrieved earlier.

I concluded that I either needed to somehow find the correct password, or modify the password to a known sequence.

Before this challenge, I was taught boolean-based blind SQL injection, so instinctively, I believed I could brute force discovery of the password by using the boolean output that the program provided me.

Entering the username "admin" always returned "That password is incorrect" (The SQL query to find a user row was TRUE) while entering a different username resulted in "The user was not found" (The SQL query to find a user row was FALSE). 

I used this boolean output in a Python script where I injected SQL in the username field to essentialy ask the program true or false questions about the password.

![alt text](<images/Screenshot 2026-01-05 at 9.10.13 PM.png>)

I first iterated through a range of numbers, injecting `' AND LENGTH(password) = {number} -- ` into the username field to find the password length, which when reached, would result in the page returning "That password is incorrect".

I found the length of the password to be `12`

Using this length, I could finally discover the password. 

![alt text](<images/Screenshot 2026-01-05 at 9.17.02 PM.png>)

By injecting `' AND BINARY SUBSTRING(password, start_position, 1) = 'random_character' -- ` I was able to brute force the password by comparing the substrings or individual characters of the password to every single possible character until I got a truthy value for each.

I discovered the password to be `TFNreE4GSNHA`

![alt text](<images/Screenshot 2026-01-05 at 9.21.38 PM.png>)

After succesfuly logging into the admin account, I was brought to a note-taking page. The page said that the flag was stored within a previously made note by another user.

I quickly observed that clicking the link to an existing note made a get request with the parameter `id`.

The note I clicked on had an id of 4, so I just changed the id to 1 in the request.

![alt text](<images/Screenshot 2026-01-05 at 9.25.52 PM.png>)

Okay, lets try 2?

![alt text](<images/Screenshot 2026-01-05 at 9.26.28 PM.png>)

Oh that was suprisingly intuitive. I found the flag by searching for a note thats not displayed on my page. I am logged in as the admin after all...
