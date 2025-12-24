# PIE TIME
 
**Date**: 12-24-2025

**Author**: Lukas Somwong  

**Platform**: PicoCTF PicoGym

**Category**: Binary

**Dificulty**: Easy

## Solution

I connected to the given program using netcat and was prompted to enter an address. Trying various addresses led to a segfault. 

![alt text](<../images/Screenshot 2025-12-24 at 3.19.12 PM.png>)

Taking a look at the source code, I could observe that I needed to execute the win() function to get the flag, but the function was not explicitly executed anywhere in the program. 

At the end of main, the address of a function input into the program would be executed. I needed to find the address of the win() function.

![alt text](<../images/Screenshot 2025-12-24 at 3.21.52 PM.png>)

Now, I couldn't just execute the program locally and copy the address of win() since the program on the remote server was compiled using PIE. The program would be loaded into a different address every time. Although, the memory address of functions within the program would always stay relative. 

Since the program gave me the adress of main() every time, I needed to find the difference between its address and the address of win()

I used GDB to disassemble the functions main() and win() from the binary, revealing the adresses of both functions.

![alt text](<../images/Screenshot 2025-12-24 at 3.33.34 PM.png>) 

![alt text](<../images/Screenshot 2025-12-24 at 3.33.51 PM.png>)

I could now find the difference between the adresses of each function.

`0133d – 012a7 = 96`

Now connected to the remote program, I subtracted the difference from the given address of the main() function to get the address of the win() function, which then caused the program to execute it and output the flag.

`5c610893033d – 96 = 5C61089302A7`

![alt text](<../images/Screenshot 2025-12-24 at 3.52.01 PM.png>)