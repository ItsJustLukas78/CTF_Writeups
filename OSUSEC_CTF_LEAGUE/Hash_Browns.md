## Hash Browns

**Date**: 9/29/25

**Author**: Lukas Somwong

**Platform**: DAMSEC CTF League

**Category**: Web

**Flags**: 1

## Overview

To find the flag, I was provided this link https://hash-browns.ctf-league.damsec.org/, which leads to a website with a password login.

A message on the website suggests that the flag is protected by a SHA-256 password.

## Solution

Following the suggestion on the website to inspect the button, I discovered that it triggered a JavaScript function called "check_password" which contained the correct password hash "d4b45864d9d6fabfc568d74f26c35ababde2105337d7af9a6605e1c56c891aa6"

Now that I had the hash on hand, I utilized the website "CrackStation," mentioned on the website, which used a hash index to determine the hash input or transformed password as "cheeseburger."

Now that I had the password on hand, I entered it into the password field on the website and clicked the submit button, which led to the page changing to display a small, bouncing text button: "Get the flag!"

Rather than attempting to click the button, I utilized the web console to call the function that the button triggered, "print_flag," resulting in the bouncing text changing into the final flag, which I copied from the HTML source file.

## Flag

osu{ch33s3bu2g3r_h45h_8r0wn5--b3773r_7h4n_y0u_7h1nk!}
