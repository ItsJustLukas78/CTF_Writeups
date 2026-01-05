# Anonymous_Images
 
**Date**: 01-04-26

**Author**: Lukas Somwong  

**Platform**: DAMSEC CTF League

**Category**: Steganography

## Overview

There are two flags to discover in this challenge. I was provided two images, flag1.png and flag2.png. 

## Solution

After downloading the images, I used exiftool to display the metadata of each image. I quickly discovered the first flag within the metadata of flag1.png under the tag "Evil Corp".

![alt text](<images/Screenshot 2026-01-04 at 9.30.41 PM.png>)

Using zsteg on aperisolve.com, I was able to get the second flag from the flag2.raw.png file.

![alt text](<images/Screenshot 2026-01-05 at 11.19.45 AM.png>)