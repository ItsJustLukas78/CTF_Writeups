# decryption-as-a-service
 
**Date**: 01-03-26

**Author**: Lukas Somwong  

**Platform**: DAMSEC CTF League

**Category**: misc

## Overview

Before this challenge, I was given a presentation about how git stores and logs data within the `.git` directory. I was then given two files: `flag.txt` which contained an encrypted flag, and `key` which was also encrypted. I was then directed to a web application posing as a ransomware attacker platform which required payment for decryption of files.

## Solution

I followed the link provided to us and discovered a `decrypt.py` file.

![alt text](<images/Screenshot 2026-02-03 at 7.20.04 PM.png>)

The `decrypt.py` file had the following contents, but it would not be succesful in decrypting our flag without an enviornmental variable I didn't have access to.

![alt text](<images/Screenshot 2026-02-03 at 7.23.38 PM.png>)

Given the context of this challenge, I checked if a `.git` directory was accesible. Appending `/.git/HEAD` to the website provided me a file containing `ref: refs/heads/master`, confirming that there was a git repository accesible to me with a `master` branch.

Going to `.git/logs/HEAD` showed the following log of commits along with their hashes

![alt text](<images/Screenshot 2026-02-03 at 6.00.42 PM.png>)

Using `git-dumper`, I created a dump of the repository on my machine so I could use `git cat-file p` to read the object files. I was able to read the `commit` object where I found the `tree` hash, then I read the `tree` object where I found the `.env` hash, and last I read the `.env` object where I found the private key used to encrypt the `key` file.

![alt text](<images/Screenshot 2026-02-03 at 6.00.05 PM.png>)

![alt text](<images/Screenshot 2026-02-03 at 5.59.55 PM.png>)

Running the `decrypt.py` file with the private key in my enviornment, I was able to decrypt the key file and then the `flag.txt` file which afterward contained the unencrypted flag.

![alt text](<images/Screenshot 2026-02-03 at 5.59.42 PM.png>)

![alt text](<images/Screenshot 2026-02-03 at 5.59.41 PM.png>)

