## Cheap Sites

**Date**: 10/11/25

**Author**: Lukas Somwong

**Platform**: DAMSEC CTF League

**Category**: Web

**Flags:** 3

## Overview

I was provided this link https://cheap-sites.ctf-league.damsec.org/ leading to a website for making room reservations. 

A file at https://chal.ctf-league.osusec.org/web/cheap-sites/server.js was provided to find flag 1. I will refer to this as File 1.

A file at https://chal.ctf-league.osusec.org/web/cheap-sites/admin.js was provided to find flag 2. I will refer to this as File 2.

No details were given on flag 3.

## Solution for Flag 1

On the website, attempting to make a room reservation leads to a prompt to complete an easy math question. Despite solving the question in a reasonable amount of time, a message declaring that I was AI for taking too long would appear, preventing me from making a reservation.

In the network tab of the browser developer tools, I could observe that a GET request at "/reserve" was being made, with the time of my response and the room number as parameters.

Inside File 1, I found the handler for the GET request and observed that a time greater than 0.125 led to a reservation being denied. 

I proceeded to use the console in the browser to reproduce the GET request with a time parameter of 0.1 which provided me the flag in the response. 

## Solution for Flag 2

In File 2, I noticed that there was an enviornment variable called "FLAG_2" which would be returned if the function getAdminPassword when the "type" parameter was "email." 

After observing the network tab from interacting with the ping button on the admin page, I figured out that I could use getAdminPassword by making a get request at the "/admin" endpoint with the query "action" set to "getAdminPassword" and query "type" to "email" which directly gave me flag 2 as a response.

## Solution for Flag 3

In File 2, I noticed that there was an enviornment variable called "FLAG_3" which would be returned if the action "sendEmail" at the "/admin" endpoint was used to send an email.

This action required the "website" password for which I only had the hash and salt for. Theoretically with enough computation and time I could crack the password, although that did not seem to be a reasonable solution.

After looking deeper, I noticed that the "grantAdmin" action had a dynamic parameter "environment" which accessed an environmental variable by the name of whatever was passed as "env" in the request, which defaulted to "NODE_ENV". I realized that if I used the query "env" and set it to "FLAG_3" in the GET request making the "grantAdmin" action, the request would respond back with the value of Flag 3.


## Flags

1: osu{fa5t3r_th4n_ai}

2: osu{th3_k1ds_w1ll_n3v3r_gu355_th1s}

3: osu{b3_c4r3ful_w1th_dyn4m1c_f13ld5}