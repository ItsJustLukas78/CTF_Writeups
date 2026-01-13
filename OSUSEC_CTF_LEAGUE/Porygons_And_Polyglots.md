# Porygons and Polyglots
 
**Date**: 01-12-26

**Author**: Lukas Somwong  

**Platform**: DAMSEC CTF League

**Category**: misc

**Flags:** 1

## Overview

In this challenge, we had one flag, but it was divided up in six fragments which were hidden accross multiple files. We were provided a single wav file.

## Solution

Playing the wav did not appear to reveal anything interesting. Using the tool `binwalk`, I could observe that there was also a zip file contained within the wav file.

![alt text](<images/Screenshot 2026-01-13 at 3.06.58 PM.png>)

Chaning the .wav extention to .zip then using the `unzip` tool revealed a folder named `super_secret_hidden_files` which contained three files called `porygon-z`, `porygon`, and `porygon2`. 

Using the `file` command, I found that the `porygon` file was a png and the `porygon2` file was a PDF document. 

![alt text](<images/Screenshot 2026-01-13 at 3.15.47 PM.png>)

Opening up these files showed an image of a pokemon and a list of pokemons, but nothing stood out to me.

Using `exiftool` on the `porygon` and `porygon-z` file revealed the second fraction of the flag in a comment `Flag Fraction 2: lvi` and a comment `Synt Senpgvba 6: crf` which when decoded with a caesar cypher revealed `Flag Fraction 6: pes`, the sixth fraction of the flag.

![alt text](<images/Screenshot 2026-01-13 at 3.32.10 PM.png>)

Using the `binwalk` command, I discovered that `porygon-z` contained a PNG, PDF, and JPEG. 

![alt text](<images/Screenshot 2026-01-13 at 3.18.23 PM.png>)

Changing the extention of `porygon-z` to .PDF allowed me to open it up as a PDF, which contained the text `Flag Fraction 5: ety`, revealing the fifth fraction of the flag.

![alt text](<images/Screenshot 2026-01-13 at 3.45.09 PM.png>)

Viewing the hexdump of `porygon2`, the PDF of pokemons, revealed javascript that printed the decoding of a base64 string. 

Using Node, I executed the following code `console.log(atob("ZmxhZyBmcmFjdGlvbiAzOiBuZ18 ="))` which revealed `flag fraction 3: ng_`, the third fraction of the flag.

![alt text](<Screenshot 2026-01-13 at 3.53.29 PM.png>)

At this point, I had half of the flag revealed: ###lving_###etypes.
Making an educated guess based on what we encoutered in this challenge, I submitted the flag `evolving_filetypes` which was correct.
