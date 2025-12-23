# JaWT Scratchpad
 
**Date**: 12-22-2025
**Author**: Lukas Somwong  
**Platform**: PicoCTF PicoGym
**Category**: Web

## Solution

Attempting to login as "admin" would result in my request getting denied. Entering other usernames such as 
"john" resulted in a cookie, "jwt", being set to a jwt token which had the payload claim "name" set to "john". 

The value of the "name" claim in the token resulted in the scratchpad for the respective user to be loaded.
The jwt needed to be modified in order for the "name" claim to be set to "admin" but the signature at the 
end of the token would be invalid since the payload had been modified.

I used jwt_tool with the rockyou.txt dictionary to discover the jwt secret to be "ilovepico".
I then resigned the modified jwt token with the known secret and set it to my cookie. Reloading the page
presented me the admin scratchpad which contained the flag.

