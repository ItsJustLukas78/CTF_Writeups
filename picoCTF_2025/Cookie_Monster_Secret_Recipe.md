# Cookie Monster Secret Recipe
 
**Date**: 12-23-2025

**Author**: Lukas Somwong  

**Platform**: PicoCTF PicoGym

**Category**: Web

**Dificulty**: Easy

## Solution

Entering any arbitrary username and password combination led to an access denied page which hinted at checking the cookies.

![alt text](<images/Screenshot 2025-12-23 at 8.52.52 PM.png>)
![alt text](<images/Screenshot 2025-12-23 at 8.53.09 PM.png>)

Using the browser developer tools showed a cookie 

`secret_recipe=cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzc4QjRDMzkwfQ==`

Decoding the cookie from base64 provided the following flag

`picoCTF{c00k1e_m0nster_l0ves_c00kies_78B4C390}`